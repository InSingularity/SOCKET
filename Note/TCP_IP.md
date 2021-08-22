# TCP/IP

### 套接字

#### 使用函数列表

```c
/**** Linux和windows相同的套接字相关函数 ****/
//创建套接字
int socket(int domain, int type, int protocol); 

//分配IP地址和端口号
int bind(iht sockfd, struct sockaddr * myaddr, socklen_t addrlen);

//转为可接受的请求状态
int listen(int sockfd, int backlog);

//受理连接请求
int accept(int sockfd, struct sockaddr * addr, socklen_t * addrlen);

//请求连接
int connect(int sockfd, struct sockaddr * serv_addr, socklen_t addrlen);

/*****  使用windos时必须以下两个函数进行winsock的初始化和注销 ****/
/*****  需要导入头文件winsock2.h，链接ws2_32.lib库			****/

//winsock初始化
//wVersionRequested:使用的Winsock版本信息。使用MAKEWORD(1,2),主版本1，副版本2
//LPWSADATA:WSADATA结构体变量的地址
int WSAStartup(WORD wVersionRequested, LPWSADATA lpWASAData);

//winsock注销
int WSACleanup(void);

/**** winsock独有 ****/

//关闭套接字
int closesocket(SOCKET s);

========================================================================
    
/**** Linux文件操作（Linux的sock操作和文件操作无区别） ****/
    
//打开文件
int open(const char * path, int flag);		//flag可以通过位运算"|"符组合并传递多个参数

//关闭文件
int close(int fd);		//文件描述符

//写入文件（Linux套接字同）
ssize_t write(int fd, const void * buf, size_t nbytes);

//读取文件 (Linux套接字同）
ssize_t read(int fd, void * buf, size_t nbytes);

/**** windows 的套接字I/O(Linux直接与文件I/O相同即:read()和write())  ****/

//winsock传输函数
int send(SOCKET s, const char * buf, int len, int flags);

//winsock接收函数
int recv(SOCKET s, const char * buf, int len, int flags);

```



#### 接受连接请求的套接字创建过程(server端)

1. 调用socket函数创建套接字。
2. 调用bind函数分配IP地址和端口号。
3. 调用listen函数转为可接受的请求状态。
4. 调用accept函数受理连接请求。



### 地址信息的表示

1. ==sockaddr_in==  (作为地址信息传递给bind函数)

   ```C
   struct sockaddr_in
   {
   	sa_family_t     sin_family;  //地址族
       unit16          sin_port;    //16位TCP/UDP端口号
       struct in_addr  sin_addr;	 //32位IP地址
       char			sin_zero[8]  //不使用
   };
   
   struct in_addr
   {
       In_addr_t 		s_addr;		//32位IPV4地址
   };
   in_addr_t	//unit32_t
   ```

2. ==sockaddr==  

   ```C
   struct sockaddr
   {
   	sa_family_t		sin_falily;		//地址族
       char			sa_data[14];	//地址信息（包含IP地址和端口号）
   };
   ```

3. 字节序转换

   3.1 大小端存储

   - 大端存储：高位字节存放在地位地址。（网络字节序选择的方式）
   - 小端存储：高位字节存放在高位地址。

   3.2 字节序转换

   

   

