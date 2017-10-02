 # UDP Socket for Server and Client 
 
1. UDP Server and Client using C++/Qt Framework
2. UDP Server and Client using Py/Qt Framework

[![Video](http://img.youtube.com/vi/YO6tPDJC4fo/0.jpg)](http://www.youtube.com/watch?v=YO6tPDJC4fo)<br>
https://www.youtube.com/watch?v=YO6tPDJC4fo

There is a library of GPIO to include in C++ and Python to interact with GPIO. Qt frame work is different from Android in which
everything is buildin. Qt creator/design allows to create window/diaglog/widget which run in event loop from this window manager we can interact with with using signals and slots. The ui file can convert into python file for PyQt using "pyuic4". PyQt has it own functions to create GUI from code. These C++ code using Qt creator to GUI but GUI for python is done by coding. It is just taking longer but the GUI looks better then convert using "pyuic4". So we can not use for loop we have to use thread timer so thread becomes the main important components to interact with GUI. 
In Qt the signal and slot like in event in Java however Qt also has event handler: whatsThis(help) is using QEvent::whatsThis, QEvent::close. Signal and slots handle click() for buttons, returnpressed by users...and events are generated by the window system or by Qt itself in response to various occurrennces it will not generate signal. We seldom need to think about events because Qt widgets emit signals when somethings significant occurs by users or readyread from socket. Events becomes useful when we write our own custom widgets or to modify the behavior of existing Qt widgets. Event like low level of signal. Events should not be confused with signals. As a rule, signals are useful when using a widget, whereas events are useful whem implemnenting a widget. For example, when we are using QPushButton, we are more interested in its clicked() signal than in the low-level mouse or key events that caused the signal to be
emitted. But if we are implementing a class such as QPushButton, we need to write code to handle mouse and key events and emit the clicked() signal when necessary. P. 224 of C++ GUI Programming - Jasmin Blanchette
This project has been used most of the stuff that Qt framework offers. Python is always faster to implement but it does not has GUI but PyQt as we see in the video the graphic is almost the same and Python can read/write into hardware like C++.

We need to aware when working with Qt Frame work:

Remember that connections are not between classes, but between INSTANCES. If you emit a signal and expect connected slots to be called, it must be emitted on an instance on which the connection was made. That's your problem. The relationship between parent and child widget.
((UDPClient*)this->parentWidget())->updateUDPClient(UDP_IP, UDP_PORT); //don't forget this object

       UDPClient *w = dynamic_cast<UDPClient *>(this->parentWidget());
       if (0 != w)
           qDebug() << " OK ------------";    // NOT 0 has parent
       else
           qDebug() << " ERROR ------------NULL POINTER"; // 0 is has no parent
           
if is has not parent we have to use something similar to this to physical connect it. Connect signal and slot, emit and execute it.
1. self.worker.updateSignal.connect(self.buttonClicked)   
2. self.updateSignal.emit(1)
3. self.worker.updateSignal.connect(self.turnLED)

 FormDialog *f = new FormDialog();  //f is obj
 connect(f, SIGNAL(updateParent(const QHostAddress&, quint16)), this, SLOT(updateUDPClient(const QHostAddress&, quint16))); 
 emit updateParent(UDP_IP, UDP_PORT);
void UDPClient::updateUDPClient(const QHostAddress& s, quint16 i)  //This is main GUI get changed

-Between show() and exed
Method 1:
    connect(ui->changeIP,SIGNAL(clicked()),f, SLOT(exec())); //either way exec() or show();
Method 2:
    f->show();
exec() blocks the application flow while show() doesn't.
exec is mainly used for modal dialogs. show() can be use for modal and modal less by setModal(true or false).

-To wrap the image we can use CSS to change it or using setPixMap. If we use set PixMap for as a button for Dialog or Label and they have no clicked function or LineEdit has no click function we can emit this using event as described above by created its own classes.

class MyLineEdit(QLineEdit):
    def __init__(self, *args):
        QLineEdit.__init__(self, *args)

    def event(self, event):
        if (event.type() == QEvent.KeyPress) and (event.key() == Qt.Key_Return):
            self.emit(SIGNAL("returnPressed"))
            return True

        return QLineEdit.event(self, event) 

class ExtendedQLabel(QtGui.QLabel): 
     def __init(self, parent):
        QLabel.__init__(self, parent)

    def mouseReleaseEvent(self, ev):
        self.emit(SIGNAL('clicked()'))

- When the system crash when parentWidget or parent the change it has no parent and pointer is point to NULL
- Change the display on main GUI using signal and slot from thread or popup dialog
- Your Worker objects 'live' in the main thread, that means all their signals will be handled by the main thread's event loop. The fact that these objects are QThreads doesn't change that. https://stackoverflow.com/questions/23718761/pyqt-signals-not-handled-in-qthread-but-in-main-thread
We need to implement like service in Android using thread and run asynchronous. On the server the socket readyread runs in the main thread will cause the deadlock if the file is too large so it needs to run its own thread so it won't hogging the main thead.
Python: concatenates a string and a number into a string and sending it using socket. http://www.tovantran.com/blog/?p=3019

<p align="center">
    <td><img src="http://www.tovantran.com/blog/wp-content/uploads/2017/09/LoopEvent-1.png" width="400" title= "Event Loop"> </td>
  </p>
