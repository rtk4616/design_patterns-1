#命令模式

#模式動機
在軟件設計中，我們經常需要向某些對象發送請求，但是並不知道請求的接收者是誰，也不知道被請求的操作是哪個，我們只需在程序運行時指定具體的請求接收者即可，此時，可以使用命令模式來進行設計，使得請求發送者與請求接收者消除彼此之間的耦合，讓對象之間的調用關係更加靈活。

命令模式可以對發送者和接收者完全解耦，發送者與接收者之間沒有直接引用關係，發送請求的對象只需要知道如何發送請求，而不必知道如何完成請求。這就是命令模式的模式動機。

#模式定義
命令模式(Command Pattern)：將一個請求封裝為一個對象，從而使我們可用不同的請求對客戶進行參數化；對請求排隊或者記錄請求日誌，以及支持可撤銷的操作。命令模式是一種對象行為型模式，其別名為動作(Action)模式或事務(Transaction)模式。



#模式結構
命令模式包含如下角色：

- Command: 抽象命令類
- ConcreteCommand: 具體命令類
- Invoker: 調用者
- Receiver: 接收者
- Client:客戶類


![](../_static/Command.jpg)


#時序圖
![](../_static/seq_Command.jpg)

#代碼分析
```cpp
#include <iostream>
#include "ConcreteCommand.h"
#include "Invoker.h"
#include "Receiver.h"

using namespace std;

int main(int argc, char* argv[])
{
    Receiver* pReceiver = new Receiver();
    ConcreteCommand* pCommand = new ConcreteCommand(pReceiver);
    Invoker* pInvoker = new Invoker(pCommand);
    pInvoker->call();

    delete pReceiver;
    delete pCommand;
    delete pInvoker;
    return 0;
}
```
```cpp
///////////////////////////////////////////////////////////
//  Receiver.h
//  Implementation of the Class Receiver
//  Created on:      07-十月-2014 17:44:02
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_8E5430BB_0904_4a7d_9A3B_7169586237C8__INCLUDED_)
#define EA_8E5430BB_0904_4a7d_9A3B_7169586237C8__INCLUDED_

class Receiver
{

public:
    Receiver();
    virtual ~Receiver();

    void action();

};
#endif // !defined(EA_8E5430BB_0904_4a7d_9A3B_7169586237C8__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  Receiver.cpp
//  Implementation of the Class Receiver
//  Created on:      07-十月-2014 17:44:02
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Receiver.h"
#include <iostream>
using namespace std;

Receiver::Receiver()
{

}

Receiver::~Receiver()
{

}

void Receiver::action()
{
    cout << "receiver action." << endl;
}
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteCommand.h
//  Implementation of the Class ConcreteCommand
//  Created on:      07-十月-2014 17:44:01
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_1AE70D53_4868_4e81_A1B8_1088DA355C23__INCLUDED_)
#define EA_1AE70D53_4868_4e81_A1B8_1088DA355C23__INCLUDED_

#include "Command.h"
#include "Receiver.h"

class ConcreteCommand : public Command
{

public:
    ConcreteCommand(Receiver* pReceiver);
    virtual ~ConcreteCommand();
    virtual void execute();
private:
    Receiver* m_pReceiver;



};
#endif // !defined(EA_1AE70D53_4868_4e81_A1B8_1088DA355C23__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteCommand.cpp
//  Implementation of the Class ConcreteCommand
//  Created on:      07-十月-2014 17:44:02
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ConcreteCommand.h"
#include <iostream>
using namespace std;


ConcreteCommand::ConcreteCommand(Receiver* pReceiver)
{
    m_pReceiver = pReceiver;
}



ConcreteCommand::~ConcreteCommand()
{

}

void ConcreteCommand::execute()
{
    cout << "ConcreteCommand::execute"  << endl;
    m_pReceiver->action();
}
```

```cpp
///////////////////////////////////////////////////////////
//  Invoker.h
//  Implementation of the Class Invoker
//  Created on:      07-十月-2014 17:44:02
//  Original author: colin
///////////////////////////////////////////////////////////

#if !defined(EA_3DACB62A_0813_4d11_8A82_10BF1FB00D9A__INCLUDED_)
#define EA_3DACB62A_0813_4d11_8A82_10BF1FB00D9A__INCLUDED_

#include "Command.h"

class Invoker
{

public:
    Invoker(Command* pCommand);
    virtual ~Invoker();
    void call();

private:
    Command* m_pCommand;


};
#endif // !defined(EA_3DACB62A_0813_4d11_8A82_10BF1FB00D9A__INCLUDED_)
```

```cpp
///////////////////////////////////////////////////////////
//  Invoker.cpp
//  Implementation of the Class Invoker
//  Created on:      07-十月-2014 17:44:02
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Invoker.h"
#include <iostream>
using namespace std;

Invoker::Invoker(Command* pCommand)
{
    m_pCommand = pCommand;
}

Invoker::~Invoker()
{

}

void Invoker::call()
{
    cout << "invoker calling" << endl;
    m_pCommand->execute();
}
```

#運行結果：

![](../_static/Command_run.jpg)


#模式分析
命令模式的本質是對命令進行封裝，將發出命令的責任和執行命令的責任分割開。

- 每一個命令都是一個操作：請求的一方發出請求，要求執行一個操作；接收的一方收到請求，並執行操作。
- 命令模式允許請求的一方和接收的一方獨立開來，使得請求的一方不必知道接收請求的一方的接口，更不必知道請求是怎麼被接收，以及操作是否被執行、何時被執行，以及是怎麼被執行的。
- 命令模式使請求本身成為一個對象，這個對象和其他對象一樣可以被存儲和傳遞。
- 命令模式的關鍵在於引入了抽象命令接口，且發送者針對抽象命令接口編程，只有實現了抽象命令接口的具體命令才能與接收者相關聯。


#實例
實例一：電視機遙控器

- 電視機是請求的接收者，遙控器是請求的發送者，遙控器上有一些按鈕，不同的按鈕對應電視機的不同操作。抽象命令角色由一個命令接口來扮演，有三個具體的命令類實現了抽象命令接口，這三個具體命令類分別代表三種操作：打開電視機、關閉電視機和切換頻道。顯然，電視機遙控器就是一個典型的命令模式應用實例。

![](../_static/Command_eg.jpg)


#時序圖:

![](../_static/seq_Command_eg.jpg)


#優點
命令模式的優點

- 降低系統的耦合度。
- 新的命令可以很容易地加入到系統中。
- 可以比較容易地設計一個命令隊列和宏命令（組合命令）。
- 可以方便地實現對請求的Undo和Redo。


#缺點
命令模式的缺點

- 使用命令模式可能會導致某些系統有過多的具體命令類。因為針對每一個命令都需要設計一個具體命令類，因此某些系統可能需要大量具體命令類，這將影響命令模式的使用。


#適用環境
在以下情況下可以使用命令模式：

- 系統需要將請求調用者和請求接收者解耦，使得調用者和接收者不直接交互。
- 系統需要在不同的時間指定請求、將請求排隊和執行請求。
- 系統需要支持命令的撤銷(Undo)操作和恢復(Redo)操作。
- 系統需要將一組操作組合在一起，即支持宏命令


#模式應用
很多系統都提供了宏命令功能，如UNIX平臺下的Shell編程，可以將多條命令封裝在一個命令對象中，只需要一條簡單的命令即可執行一個命令序列，這也是命令模式的應用實例之一。


#模式擴展
宏命令又稱為組合命令，它是命令模式和組合模式聯用的產物。

-宏命令也是一個具體命令，不過它包含了對其他命令對象的引用，在調用宏命令的execute()方法時，將遞歸調用它所包含的每個成員命令的execute()方法，一個宏命令的成員對象可以是簡單命令，還可以繼續是宏命令。執行一個宏命令將執行多個具體命令，從而實現對命令的批處理。

#總結
- 在命令模式中，將一個請求封裝為一個對象，從而使我們可用不同的請求對客戶進行參數化；對請求排隊或者記錄請求日誌，以及支持可撤銷的操作。命令模式是一種對象行為型模式，其別名為動作模式或事務模式。
- 命令模式包含四個角色：抽象命令類中聲明瞭用於執行請求的execute()等方法，通過這些方法可以調用請求接收者的相關操作；具體命令類是抽象命令類的子類，實現了在抽象命令類中聲明的方法，它對應具體的接收者對象，將接收者對象的動作綁定其中；調用者即請求的發送者，又稱為請求者，它通過命令對象來執行請求；接收者執行與請求相關的操作，它具體實現對請求的業務處理。
- 命令模式的本質是對命令進行封裝，將發出命令的責任和執行命令的責任分割開。命令模式使請求本身成為一個對象，這個對象和其他對象一樣可以被存儲和傳遞。
- 命令模式的主要優點在於降低系統的耦合度，增加新的命令很方便，而且可以比較容易地設計一個命令隊列和宏命令，並方便地實現對請求的撤銷和恢復；其主要缺點在於可能會導致某些系統有過多的具體命令類。
- 命令模式適用情況包括：需要將請求調用者和請求接收者解耦，使得調用者和接收者不直接交互；需要在不同的時間指定請求、將請求排隊和執行請求；需要支持命令的撤銷操作和恢復操作，需要將一組操作組合在一起，即支持宏命令。

