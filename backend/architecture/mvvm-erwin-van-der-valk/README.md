# MVVM - Erwin Van Der Valk's blog: Practicing patterns

[back](../README.md)

## How do I adapt a simple vs complex model

### What is that model thingy?

A model is more than just a POCO class that holds data. I view the collection of classes that solve a particular business problem. This can include:

- A business entity that stores the data.
- A Business component that holds business logic and operates on top of a business entity.
- Classes that retrieve or persist the data, for exmple repositories, services agents, etc.

The classic Model View Controller paradigm can be thought of as batch processing: The controller handles the input from the user and how to respond to that input. The view handles everything related to showing the output to the user. So the model encompasses the data and all the processing that is done with that data.

If you modeled your Model around your UI, then you have to change your Model when the UI changes. When you have designed you Model around the business logic, then your ViewModel can adapt the model to the process the user follows. Now of course there are tradeoff's. If you have very little business logic, and the UI is the only consumer of the business logic, then it might make more sense to design the model to more closely match what you see on the screen.

### Working with "easy to use" models

Sometimes, you have the fortune to get really easy to work with models. Easy to use models typically have the following characteristics:

- **They are easy to bind to** - They implements `INotifyPropertyChanged` for properties, `ICollectionNotifyPropertyChanged` for all collections of objects, and validations logic implementing `IDataErrorInfo`.
- **The model corresponds closely to what you see on the screen** - If the model is vey similar to the data on the UI.

So there are a couple of things you can do when you work with easy to bind models:

- **Consuming the model directly from the View** - If your Model exposes most of the data your view needs, then the view can directly consume the model.
- **Don't blindly create ViewModels for all of your view** - If you don't need one, because there is no Logic or UI state to hold, then don't create one.

![Simple Model MVVM](./assets/simple-model-mvvm.png "simple-model-mvvm")

### Working with "hard to use" models

A hard to wok with model has the following characteristics:

- The model does not easily map to waht you need in the UI. For example, the view needs to combine information from several models.
- It does not allow two-ways data binding.

![Complex Model MVVM](./assets/complex-model-mvvm.png "complex-model-mvvm")

## Resources

- [How do I adapt a simple vs complex model](https://docs.microsoft.com/en-us/archive/blogs/erwinvandervalk/how-do-i-adapt-a-simple-vs-complex-model)
- [Implementing the Model View ViewModel pattern](https://docs.microsoft.com/en-us/archive/blogs/erwinvandervalk/implementing-the-model-view-viewmodel-pattern)

[back](../README.md)
