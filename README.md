# SimpleIM
于2018年8月份时用纯C编写的即时通讯系统，以示纪念。
    使用win32api实现了客户端GUI和socket功能，暂时没有群聊功能。当时知识水平有限，为了尽快实现功能，只使用了简单的socket通信，每一个在线用户分配一个单独的线程，并且当时没有顾及代码风格，使用了大量的全局变量以及判断语句，也没有进行适量的注释，导致后来我自己都有点阅读困难。。。
    有好友管理和查询用户以及对话记录载入功能，都是用文件来实现。注册用户的账号密码都存放在服务端的文本文件中，服务端也用每个用户名命名一个文件用来存放好友数据，聊天记录通过服务端以及本地文本文件共同实现。当初实现方法大部分是自己瞎想，可能有很多不合理之处。
   该项目基于 C/C++，使用 win32API，实现局域网内的即使通讯。采用可靠传输协议 TCP，i/o 多路复用 select 模型。系统采用客户机/服务器架构模式通过 Windows 提供的 Socket 来连接客户机和服务器并使客户机和服务 器之间相互通信，服务器端设计与实现过程中，采用了多线程技术，执行不同的任务。聊天系统完成后将可进行 多人对多人的聊天，对好友进行添加、删除，对新用户的注册，发送消息、接受消息等功能。

# SimpleIM
＃自定义协议  
#ifndef __PROTO_H__
#define __PROTO_H__

#define PROTO_NULL 0

struct USER_BASEINFO 
{
	UINT m_uID;
	TCHAR m_szNickName[20];
	TCHAR m_szSex[20];
};

struct USER_INFO 
{
	USER_BASEINFO BaseInfo;

	TCHAR m_szPswd[20];
};


//////////////////////////////////////////////////////////////////////////
//register
//////////////////////////////////////////////////////////////////////////
//register protocol
#define REGISTER_REQUEST 10			//client request
#define REGISTER_RESPONSE 11		//service response

//register struct
struct REGISTER_PACK 
{
	UINT uProtoID;
	//ID, password, sex, age...
	USER_INFO m_UserInfo;	
};

struct RESPONSE_PACK 
{
	UINT uProtoID;					//service 
	int nResponse;
};


//////////////////////////////////////////////////////////////////////////
//login 20
//////////////////////////////////////////////////////////////////////////
#define LOGIN_REQUEST 20			//client
#define LOGIN_RESPONSE 21			//service
#define NOTIFY 22					

struct LOGIN_PACK 
{
	UINT uProtoID;
	//ID, password
	UINT m_uID;
	TCHAR m_szPswd[20];
};

struct NOTIFY_PACK 
{
	UINT uProtoID;
	//ID, state
	UINT m_uID;
	UINT m_uState;
};

//struct RESPONSE_PACK 
//{
//	UINT uProtoID;					//service 
//	int nResponse;
//};

//////////////////////////////////////////////////////////////////////////
//modify 30
//////////////////////////////////////////////////////////////////////////
//modify protocol
#define MODIFY_REQUEST 30
#define MODIFY_RESPONSE 31

//modify struct

struct MODIFY_PACK 
{
	UINT uProtoID;
	USER_INFO m_UserInfo;
};

//////////////////////////////////////////////////////////////////////////
//friend 40
//////////////////////////////////////////////////////////////////////////
//friend protocol
#define FRIEND 40			//Flag:0-Add, 1-Delete, 2-info
#define FRIEND_FIND 41
#define FRIEND_ADD 42
#define FRIEND_INFO 43
#define FRIEND_LIST 44

struct FRIEND_REQUEST 
{
	UINT uProtoID;

	UINT m_uFlag;
	UINT m_uID;
	UINT m_uFriendID;
};

struct FRIENDFIND_PACK 
{
	UINT uProtoID;

	USER_BASEINFO UserInfo;
};

struct FIND_ADD_PACK
{
	UINT uProtoID;

	UINT m_uID;
	UINT m_uFriendID;
	int m_nState;
};

struct FRIEND_LIST_PACK 
{
	UINT uProtoID;

	int m_nCount;
	TCHAR m_szFriendList[1024];
};

//////////////////////////////////////////////////////////////////////////
//chat 50
//////////////////////////////////////////////////////////////////////////
//chat protocol
#define CHAT 50

//chat struct
struct CHAT_PACK 
{
	UINT uProtoID;

	UINT m_uUserID;
	UINT m_uFriendID;
	TCHAR m_szChat[255];
};


#endif
