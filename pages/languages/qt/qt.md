Qt Basics
=========

Organizing Projects
-------------------

If you use Qt Creator, it will create a stub project for you. However,
we are going to make a slight change or two. First, we are going to
create a main controller class to own the MainView, any models
associated with it, etc. This controller will own the MainView, as well
as the models. It will be responsible for calling show on the MainView
instance, as well as manage its lifecycle. This can be achieved by
holding unique~ptrs~ to owned instances, or implementing more
traditional RAII with a destructor. One other point. The MainWindow and
owned models constructors will not have to accept a parent pointer. They
will set their superclass parents as nullptr since the controller will
be responsible for lifecycle.

So, here is what we are going to do:

-   follow a model / view / controller paradigm
    -   ui files and implementation go into model/
    -   QObject models go into model/
    -   QObject parent goes into controller/
-   controller
    -   implements void show()
    -   constructor sets up signals/slots
    -   holds unique_ptr to model and view instances
-   View class
    -   implements a default constructor and initializes its superclass
        with nullptr
-   Model class(es)
    -   inherit from QObject
    -   implement default constructors
    -   initialize their superclass to nullptr
