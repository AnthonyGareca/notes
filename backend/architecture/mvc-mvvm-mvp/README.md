# MVC - MVVM - MVP

[back](../README.md)

## MVC - Model, View, Controller Pattern

MVC lets you build an application with SoC (Separations of Concerns) that, in turn, eases the efforts to test, maintain and extend the application. In a traditional software development procedure, we write a relevant code or use user control to make the view part of definition class. This procedure increases the size of view class among business operations, data binding logic, and UI. Thus, the MVC architecture pattern is designed to reduce the code size and make a code - good code. Cleaner and easily manageable.

### Model

It characterizes a set of classes to describe business logic. It also outlines the business rules for data for data on how data can be handled or changed.

### View

It stands for UI components such as jQuery, HTML, CSS, etc. The view is responsible for displaying the data that is received from the controller as a result. View is also used to convert the model into the UI.

### Controller

It is highly responsible for proceeding with requests. Controller gets the user's data through the model to view. A controller performs as a facilitator between the model and the view.

Its responsibility is to process incoming requests. It gets input from user via the View, then processes the user's data through the model, passing back the results to view. It normally acts as a mediator between the view and the model.

### Advantages of MVC pattern

* It backups async techniques
* Modifications done doesn't influence the model
* Development is faster compared to other architectures

### Disadvantages of MVC pattern

* Not compatible with TDD
* It is challenging to manage controller form more extensive code

## MVP - Model, View, Presenter Pattern

In this architecture pattern, "p" stands for the presenter. The view manages and display the page controls. The presenter is accountable for addressing all the UI events on behalf of the view. It collects input from the users then proceed the data from side to side the model hat transform the results back to the view.

The presenter mainly performs from the logic end for gestures such as push a button or directs roads through navigation. MVP is a compound pattern to implement, but it is beneficial if applied as a well-designed solution. MVP pattern is usually performed with windows form applications and ASP.NET.

### Model

It is a set of classes to describe business logic. It also outlines the business rules for data on how data can be handled or changed.

### View

The view stands for UI components such as jQuery, HTML, CSS, etc. The view is accountable for displaying the data which is received from the controller as an outcome. It also transforms models into the UI.

### Presenter

It takes responsibility to address all UI events on behalf of the view. The view provides input from the user, then take the help of model to filter data and then convey the result to the view. The view and presenter are entirely distinct but communicate with each other through an interface.

It is responsible for addressing all user interface events on behalf of the view. It receives input from users view the view, then process the user's data through the model that passes the results back to the view. The view and the presenter are completely separated, unlike view and controller, from each other and communicate to each other by an interface. The presenter also doesn't handle the incoming request traffic like controller.

### Advantages of MVP pattern

* The view is made passive so that you can easily swap
* code is easier to manage
* compatible with TDD whn compared to MVC

### Disadvantages of MVP pattern

* It doesn't support loose coupling between view and presenter
* It has a vast code size

## MVVM - Model, View, ViewModel Pattern

MVVM supports two-way data binding between new and view-model. It allows automatic propagation of modifications inside view-model to the view. Usually, the view-model makes use of observer patterns to make changes in the view-model to the model.

### Model

It is a set of classes to describe business logic. It also outlines the business rules for data on how data ban be handled or changed. It is directly related to database activities.

### View

The view stand for UI components such as jQuery, HTML, CSS, etc. The view is accountable for displaying the data which is received from the controller as an outcome. It is the only user-interactive thing available. It also transforms models into the UI. The view in MVVM is active compared to MVC and MVP.

### ViewModel

The view-model is accountable for presenting functions, methods, and commands to uphold the state of the view, operate the model, and activate the events in the view itself. It can be defined as the model for the application.

It is responsible for displaying methods, commands, and other functions that assist in maintaining the state of the view, manipulating the model as the result of actions on the view, and triggering the events in the view itself.

### Advantages of MVVM pattern

* Loose coupling between view and view-model
* Provides the best compatibility with TDD

### Disadvantages of MVVM pattern

* Each UI component needs observables
* It has a vast code size


[back](../README.md)
