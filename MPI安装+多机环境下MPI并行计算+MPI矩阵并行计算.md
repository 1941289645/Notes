# 实验内容
1. 创建多进程，输出进程号和进程数
2. 运行多进程并行例子程序
3. 编程实现大规模向量/矩阵并行计算
# 一、MPI的下载与安装(三台虚拟机都要配)
1、在开始安装之前，先检查一下是否已经安装好了相应的编译器。
~~~
which gcc 
which gfortran 
~~~
2、安装MPICH之前，首先要在centos6.5上安装c编译器，（进入超级用户）使用指令安装如下：
~~~
yum install gcc ///安装GCC编译器（支持C编译）
yum install gcc-c++ ///安装G++编译器（支持C++编译）
~~~
3、（返回普通用户）将下载的程序安装包放在主机的某个文件夹下，在这里我新建了一个文件夹/home/lhc/mpi，文件压缩包放在mpi文件下.
使用pwd可以查看当前目录
文件压缩包名为：openmpi-1.6.5.tar.gz
4、新建一个文件夹，用于存放安装路径
~~~
mkdir /home/lhc/mpi/mpi-install
~~~

5、进入压缩文件夹的存放目录 cd /home/lhc/mpi，解压文件：tar -xzvf openmpi-1.6.5.tar.gz

6、系统配置
~~~
cd openmpi-1.6.5#进入解压后的文件夹内 
~~~
若当前使用得Shell的辅助检索路径中没有设置当前目录，则应使用命令
~~~
 ./configure --prefix=/home/lhc/mpi/mpi-install 
~~~
注：/home//lhc/mpi/mpi-install是安装路径
此外在配置过程可以指定编译器或选择用rsh或ssh（忽略）。

7.编译：make

8.安装：make install

9.设置路径：
普通用户安装，可以统一添加一条辅助检索路径。方法为修改用户目录下的：~/.bash_profile文件，运行：
~~~
vim ~/.bashrc
~~~
在适当位置修改和添加：
~~~
export PATH=/home/lhc/mpi/mpi-install/bin:$PATH
export INCLUDE=/home/lhc/mpi/mpi-install/include:$INCLUDE
export LD_LIBRARY_PATH=/home/lhc/mpi/mpi-install/lib:$LD_LIBRARY_PATH
~~~
保存退出之后 ，使用source这一命令执行一下就把新加的命令执行了。
~~~
source ~/.bashrc 
~~~
我是先使用普通用户安装，然后再进入超级用户配置一下路径：
统一添加一条辅助检索路径。方法为修改**vim /etc/profile**文件在适当位置修改和添加：
~~~
export PATH=/opt/openmpi-1.6.5/bin:$PATH
export INCLUDE=/opt/openmpi-1.6.5/include:$INCLUDE
export LD_LIBRARY_PATH=/opt/openmpi-1.6.5/lib:$LD_LIBRARY_PATH
~~~
运行以下命令使修改生效：
~~~
source /etc/profile
~~~
继续修改：
~~~
	vim /root/.bashrc
~~~
在适当位置修改和添加：
~~~
export PATH=/home/lhc/mpi/mpi-install/bin:$PATH
export INCLUDE=/home/lhc/mpi/mpi-install/include:$INCLUDE
export LD_LIBRARY_PATH=/home/lhc/mpi/mpi-install/lib:$LD_LIBRARY_PATH
~~~
执行生效：
~~~
source /root/.bashrc
~~~

10、使用下列命令来查看程序安装路径（普通和超级用户都能检测到才行，如果超级用户不能，那从超级用户上面第六步开始再试试）
~~~
which mpicc 
which mpiexec 
~~~
之后，用which来检验下配置的环境变量是否正确。如果显示了其路径，则说明安装顺利完成了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/009ff910b6b14ed18add8c2df1943053.png)


查看mpi版本：
~~~
Mpiexec --version 
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/aff20f9a8d32403ab06a2ef926d80225.png)

11.单节点测试
使用mpi编程进入例程文件夹 cd examples
这时候，进入到最开始解压的文件夹中，到解压的文件夹内的examples文件夹中，测试一下hello是否能顺利运行。
![在这里插入图片描述](https://img-blog.csdnimg.cn/1f5d55a0f1254e0da5f531fbe987a4d9.png)

编译运行：
	一般情况下，管理结点只进行程序编译与提交任务，不建议在管理结点运行计算程序，下面为测试mpich是否安装成功，使用管理节点单个节点运行MPI程序。
~~~
mpicc -o hello_c hello_c.c
mpiexec -np 4 ./hello_c
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/496ed6b66e7b4e0a941d8b856598cf22.png)

# 二、 运行MPI示例程序
首先要实现三台虚拟机免密登录，在之前已经实现，这里不做陈述（注意只有root才能免密登录、必要）
## 1、配置NFS共享目录安装配置
服务端：master(192.168.75.130)
客户端：slave1(192.168.75.129)、slave2(192.168.75.131)
### 1.1 服务端配置
1、查看是否已经安装nfs（以下配置都在root用户，三台机子都在）
~~~
rpm -qa |grep nfs
~~~
2、没有安装则进行安装（root用户）
~~~
yum -y install nfs-utils rpcbind
~~~
3、创建共享目录，一般在根目录下进行创建
~~~
mkdir /home/lhc/mpi/cloud
~~~
服务端与客户端两者文件位置与名称应一致，方便后续运行mpi
4、配置/etc/exports
~~~
vim /etc/exports
~~~
输入
~~~
 /home/lhc/mpi/cloud *(rw,sync,no_root_squash)
~~~
可用此行代替以下两行，*代表所有节点，建议使用上面一行
~~~
/home/lhc/mpi/cloud 192.168.75.129(rw,sync,no_root_squash)
/home/lhc/mpi/cloud 192.168.75.131(rw,sync,no_root_squash)
~~~
**no_root_squash**：当登录NFS主机使用共享目录的使用者是root时，其权限将被转换成为匿名使用者，通常它的UID与GID都会变成nobody身份。
**root_squash**；如果登录NFS主机使用共享目录的使用者是root，那么对于这个共享的目录来说，它具有root的权限。
5、重启nfs服务（第一次跳过此行，如遇到错误修改上面文件时，使用此重启服务）：
~~~
service nfs restart
~~~
6、启动服务并设置开机启动
~~~
service rpcbind start
service nfs start
chkconfig --level 2345 rpcbind on
chkconfig --level 2345 nfs on
~~~
### 1.2 客户端配置
1、查看是否已经安装nfs
~~~
rpm -qa |grep nfs
~~~
2、没有安装则进行安装（root用户）
~~~
yum -y install nfs-utils rpcbind
~~~
3、查看服务端共享目录
~~~
showmount -e 192.168.75.130
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc0f0ba56ccb40eb9fd1a0e8c9f17e05.png)

5、挂载共享目录（需现在相同本地文件夹mpi内创建相同文件夹cloud）到本地，并测试
~~~
mount -t nfs 192.168.75.130:/home/lhc/mpi/cloud /home/lhc/mpi/cloud
~~~
df -h 查看是否挂载成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/e43aeee33acd43929ea02d3a0e7d9c9c.png)


6、设置开机自动挂载
~~~
vim /etc/fstab
~~~
输入
~~~
192.168.75.130:/home/lhc/mpi/cloud /home/lhc/mpi/cloud nfs defaults 0 0
~~~
## 2、运行test.cpp
默认将编译成功的可执行文件(本案例文件名为test.cpp)置于共享文件夹cloud中，移动至该目录下执行：
~~~
mpic++ test.cpp -o test
mpirun -np 4 -host slave1,slave2 ./test
~~~
其中:
**--np 4**：表明调用4个进程
**--host slave1,slave2**：指定两台机器的IP别名
运行vim /etc/hosts，给IP地址赋别名（此文件应该在实现免密登录时就已经设置过此文件）
![在这里插入图片描述](https://img-blog.csdnimg.cn/e58db57d08b547e2933ecaff2f5bf19d.png)

运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/6c32992a616141d596e02b5de6e5f858.png)


test.cpp:
~~~cpp
#include <mpi.h>
#include <iostream>
#include <stdlib.h>
int main(int argc, char *argv[]){
  int size,myid, inside=0, outside=0, points=10000;
  double x,y;
  double start, end;

  MPI_Init(&argc, &argv);

  MPI_Comm_rank(MPI_COMM_WORLD, &myid);
  MPI_Comm_size(MPI_COMM_WORLD, &size);



  srand(time(0)+myid);

  MPI_Barrier(MPI_COMM_WORLD);
  start=MPI_Wtime();

  double* rands=new double[2*points];

  for(int i=0; i<2*points; i++)
  {
    rands[i]=rand();
  }

  for(int i=0; i<points; i++)
  {
    x=rands[i*2]/RAND_MAX;
    y=rands[i*2+1]/RAND_MAX;
    if ((x*x+y*y)<1) ++inside;
  }

  delete[] rands;
 for (int i=1; i<size; ++i) {
    if (myid==0){
      int temp;
      MPI_Recv(&temp, 1, MPI_INT, i, i, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
      inside+=temp;
    }
    else {
      MPI_Send(&inside, 1, MPI_INT, 0, i, MPI_COMM_WORLD);
    }
  }

  MPI_Barrier(MPI_COMM_WORLD);
  end=MPI_Wtime();

  if (myid==0)
  {
    double pi_monte= 4*(double)inside / (double)(size*points);    	printf("pi= %0.11f\n",pi_monte);
    printf("the time is %0.6f\n",(end-start));
  }

  MPI_Finalize();
}
~~~
## 3、运行mpi3.c
另一个例子程序，命名为mpi3，程序如下：
~~~cpp
#include "mpi.h"
#include <time.h>
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char** argv){
    int N = 100000000;
    int a = 0, b = 10, i;
    int proc_id, proc_num;
    double local = 0, total = 0, recv, x, dx = (double)(b-a)/N;
    MPI_Status status;
    MPI_Init(&argc, &argv); 
        MPI_Comm_size(MPI_COMM_WORLD, &proc_num); 
        MPI_Comm_rank(MPI_COMM_WORLD, &proc_id); 
	local = 0;
     
        for (i = proc_id; i < N;i += proc_num){
            x = a + i * dx + dx / 2;
	    local += x*x*dx;
        }
	if(proc_id != 0){ 
		printf("Sending result %lf from %d to 0\n", local, proc_id);
		MPI_Send(&local, 1, MPI_DOUBLE, 0, 1, MPI_COMM_WORLD);
	}
	else{ 
		for(i = 1; i < proc_num; i+=1){
			MPI_Recv(&recv, 1, MPI_DOUBLE, i, 1, MPI_COMM_WORLD, &status);
			total += recv;
		}
		total += local;
		printf("Result: %lf\n", total); 
	}
    MPI_Finalize(); 
    return 0;
}
~~~
将其保存在共享文件夹cloud中，移动至该目录下执行：
~~~
mpicc mpi3.c -o mpi3
mpirun -np 5 ./mpi3
~~~
此时是测试单机运行，没有问题：
![在这里插入图片描述](https://img-blog.csdnimg.cn/932734474c824e29988eea33b472f3f4.png)


接下来测试分配给多机运行：
用一个hostfile文件来为两节点指定slot数量：
~~~
vim hostfile
~~~
在打开的新文件里输入：
~~~
slave1 slots=5
Slave2 slots=5
~~~
保存并退出，执行指定了hostfile的mpirun:
~~~
mpirun -host slave1,slave2 --hostfile hostfile -np 10 mpi3
~~~
# 三、矩阵并行计算
~~~
mpic++ martrix.cpp -o martrix
mpirun -np 8 -host slave1,slave2 ./martrix 300 200 400
~~~
~~~cpp
#include<iostream>
#include<mpi.h>
#include<math.h>
#include<stdlib.h>

void initMatrixWithRV(float *A, int rows, int cols);
void matMultiplyWithSingleThread(float *A, float *B, float *matResult, int m, int p, int n);
int main(int argc, char** argv)
{
    int m = atoi(argv[1]);
    int p = atoi(argv[2]);
    int n = atoi(argv[3]);
    
    float *A, *B, *C;
    float *bA, *bC;  

    int myrank, numprocs;

    MPI_Status status;
  
    MPI_Init(&argc, &argv);  // 并行开始
    MPI_Comm_size(MPI_COMM_WORLD, &numprocs); 
    MPI_Comm_rank(MPI_COMM_WORLD, &myrank); 
    
    int bm = m / numprocs;

    bA = new float[bm * p];
    B  = new float[p * n];
    bC = new float[bm * n];

    if(myrank == 0){
        A = new float[m * p];
        C = new float[m * n];
        
        initMatrixWithRV(A, m, p);
        initMatrixWithRV(B, p, n);
    }
    MPI_Barrier(MPI_COMM_WORLD);
　　/* step 1: 数据分配 */
    MPI_Scatter(A, bm * p, MPI_FLOAT, bA, bm *p, MPI_FLOAT, 0, MPI_COMM_WORLD);
    MPI_Bcast(B, p * n, MPI_FLOAT, 0, MPI_COMM_WORLD);
　　/* step 2: 并行计算C的各个分块 */
    matMultiplyWithSingleThread(bA, B, bC, bm, p, n);
    
    MPI_Barrier(MPI_COMM_WORLD);
    
　　/* step 3: 汇总结果 */
    MPI_Gather(bC, bm * n, MPI_FLOAT, C, bm * n, MPI_FLOAT, 0, MPI_COMM_WORLD);
  
　　/* step 3-1: 解决历史遗留问题（多余的分块） */
    int remainRowsStartId = bm * numprocs;
    if(myrank == 0 && remainRowsStartId < m){
        int remainRows = m - remainRowsStartId;
        matMultiplyWithSingleThread(A + remainRowsStartId * p, B, C + remainRowsStartId * n, remainRows, p, n);
    }
  
    delete[] bA;
    delete[] B;
    delete[] bC;
    
    if(myrank == 0){
        delete[] A;
        delete[] C;
    }
    
    MPI_Finalize(); // 并行结束

    return 0;
}
void initMatrixWithRV(float *A, int rows, int cols)
{
    srand((unsigned)time(NULL));
    for(int i = 0; i < rows*cols; i++){
        A[i] = (float)rand() / RAND_MAX;
    }
}void matMultiplyWithSingleThread(float *A, float *B, float *matResult, int m, int p, int n)
{
    for(int i=0; i<m; i++){
        for(int j=0; j<n; j++){
            float temp = 0;
            for(int k=0; k<p; k++){
                temp += A[i*p+k] * B[k*n + j];
            }
            matResult[i*n+j] = temp;
        }
    }
}
~~~