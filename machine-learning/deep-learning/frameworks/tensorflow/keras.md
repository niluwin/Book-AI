# Keras

## Keras Model Life-Cycle

1. Define Network. 
2. Compile Network. 
3. Fit Network. 
4. Evaluate Network. 
5. Make Predictions.

### Step 1. Define Network

Neural networks are defined in **Keras** as a sequence of layers. The container for these layers is the Sequential class. Create an instance of the **Sequential** class. Then you can create your **layers** and add them in the order that they should be connected. For example, we can do this in two steps:

```text
...
model = Sequential()
model.add(Dense(2))
```

But we can also do this in one step by creating an array of layers and passing it to the constructor of the **Sequential** class.

```text
...
layers = [Dense(2)]
model = Sequential(layers)
```

**The first layer in the network must define the number of inputs to expect.** The way that this is specified can differ depending on the network type, but for a Multilayer Perceptron model this is specified by the input dim attribute.

_For example, a small Multilayer Perceptron model with 2 inputs in the visible layer, 5 neurons in the hidden layer and one neuron in the output layer can be defined as:_

```text
...
model = Sequential()
model.add(Dense(5, input_dim=2))
model.add(Dense(1))
```

Think of a Sequential model as a pipeline with your raw data fed in at the bottom and predictions that come out at the top. 

This is a helpful conception in Keras as components that were traditionally associated with a layer can also be split out and added as separate layers, clearly showing their role in the transform of data from input to prediction.

_For example, activation functions that transform a summed signal from each neuron in a layer can be extracted and added to the Sequential as a layer-like object called the **Activation** class._

```text
...
model = Sequential()
model.add(Dense(5, input_dim=2))
model.add(Activation(✬relu✬))
model.add(Dense(1))
model.add(Activation(✬sigmoid✬))
```

