---
title: "[ARCHIVED] Learning High School Physics with Tensorflow"
date: 2018-05-28T17:54:04+01:00
draft: false
---
<head>
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>

</head>
## Introduction
Learning to convert from Angular displacement to linear displacement was nothing fancy in highschool, but TEACHING a machine learning model how to do the translation is on a whole new level <!-- more -->. Just kidding, it's also nothing fancy.


## Task
What we want to do is learn the mapping between the angular rotation (complete circle) of a circular object given its diameter to the distance covered on a linear path


So we want to find the length of the underlying line that the circle covers while completing a full rotation.


## What we already know

From highschool, we already know the formula to calculate this function.
<br>
<div style="text-align: center;">
    $y = 2 \times \Pi \times r$
</div>
<br>

which yields to
<br>
<div style="text-align: center;">
$y = 6.283185 \times r$
</div>
<br>


So we want our Neural network to learn the parameter `a = 6.283185`  

## Dataset

Since we already have the mapping from input to output, then we can generate the dataset ourselves.

```python
import numpy as np 
import math 
dataset_size = 200

def generate_dataset():
  
  radii = np.random.uniform(-20, 20, size=dataset_size)
  np.random.shuffle(radii)
  distance_covered_by_one_lap =  2.0 * math.pi * radii

  return radii , distance_covered_by_one_lap
```

> We are going to consider the distance covered by 1 full rotation of circles with different radii

Since the function is linear, then the curve will be a straight line.




<p style="overflow-x:scroll;" id="dataset" style="width:100%;height:400px;"></p>
<script src="https://cdn.plot.ly/plotly-latest.min.js">
  var graph = document.getElementById('dataset');
  var trace1 = {
    x: [  4.37820605, -13.13363508,  -2.77673978,   2.4888348 ,
        17.0341001 ,  -1.93372242, -19.9116696 ,  -5.18060901,
        -3.40557481,   3.20888519, -12.43556159, -10.69098233,
        11.52953166,  18.63755458, -17.64877663,  17.58287248,
        14.54411441,   3.28402567,   0.92767511,  -9.58355823,
         1.83476079,  -1.32374586,   2.29633481,   5.92822859,
         4.95727362,   4.54170041,  -8.72402561,  -0.48569011,
        19.84503662, -15.79886948,  13.49928393, -10.34547939,
        -4.78252249, -15.70707044,  -7.76520545,  -0.32884734,
        13.73167646, -10.98187383, -19.95463039, -10.75829183,
         5.45210465,   1.59442852, -11.85793133,  -8.96219708,
        -7.08803902,   3.85229111, -15.57103688,  15.63200638,
        13.34456157, -14.20435259, -15.91847545,  11.60423062,
       -18.48668068, -19.34898864,  18.25889563,   3.19425834,
       -19.86038836,   4.53078929,  -3.7072689 ,  -4.02429871,
        -1.76539668,  -5.8335618 ,  -5.50317684,  17.69807207,
        -4.87084594,  -1.28215731, -15.78324596,  -5.42589011,
        19.78377538,  -5.96690856, -17.60933395,   1.41597035,
         2.00853934,  -9.58346522,  -2.78490348,   8.23029841,
        10.18045946, -12.08878606,  18.9445205 ,  13.42463006,
        -4.93140455,   0.11713642,  14.84123014,  19.07327806,
        16.0601094 ,  -0.37356727,   2.89174422,  -8.32691051,
         4.83827075,  14.30138207, -15.03557732,  -7.20179809,
        -6.8465606 ,  18.83365751,  -7.16707826,  18.03415001,
         9.96512857,  -2.46091803, -10.93929805,  15.91218687],
    y: [  27.50907993,  -82.52106297,  -17.44677058,   15.63781024,
        107.02840745,  -12.14993628, -125.10870985,  -32.55072644,
        -21.39785758,   20.16202027,  -78.13493786,  -67.17342311,
         72.4421839 ,  117.10320911, -110.89053402,  110.47644605,
         91.38336598,   20.63414185,    5.82875459,  -60.21527228,
         11.52814203,   -8.31734052,   14.42829711,   37.2481588 ,
         31.14746877,   28.53634527,  -54.81466951,   -3.05168096,
        124.6900425 ,  -99.26722456,   84.81850243,  -65.00256412,
        -30.04947506,  -98.69043418,  -48.79022476,   -2.0662088 ,
         86.27866778,  -69.0011483 , -125.37864046,  -67.59634117,
         34.25658384,   10.01808988,  -74.50557994,  -56.311145  ,
        -44.5354626 ,   24.20465892,  -97.83571017,   98.21879283,
         83.84635318,  -89.24857948, -100.01873104,   72.91153132,
       -116.1552404 , -121.57328115,  114.72402475,   20.07011708,
       -124.78650033,   28.46778872,  -23.29345747,  -25.28541451,
        -11.09231449,  -36.65334981,  -34.57747986,  111.20026641,
        -30.60442764,   -8.05603195,  -99.16905913,  -34.09187302,
        124.30512681,  -37.4911922 , -110.64270834,    8.8968041 ,
         12.62002485,  -60.21468786,  -17.49806463,   51.71249005,
         63.96571329,  -75.95608297,  119.03193285,   84.34943832,
        -30.98492859,    0.73598982,   93.25019917,  119.84094046,
        100.90864343,   -2.34719237,   18.1693648 ,  -52.31952174,
         30.39975171,   89.85823368,  -94.47131847,  -45.25023193,
        -43.01820896,  118.33536014,  -45.0320808 ,  113.31190634,
         62.6127494 ,  -15.46240401,  -68.73363678,   99.97921876],
    mode: 'markers',
    type: 'scatter'
  };
  Plotly.newPlot('dataset', [trace1]);
</script>


## Model

We are going to use a very simple neural network. Actually it's not going to be a network, just 1 neuron.




## Code
```python
X = tf.placeholder(dtype=tf.float32,shape=(dataset_size,),name = 'Input_X')
Y = tf.placeholder(dtype=tf.float32,shape=(dataset_size,),name = 'Y')

def step(X, weight):
    h = tf.multiply(X,weight)
    a = tf.nn.relu(h)
    return a

yhat = step(X,w)

cost = tf.reduce_sum(tf.pow(tf.subtract(yhat,Y), 2))/(dataset_size)
train = tf.train.GradientDescentOptimizer(learning_rate=lr).minimize(cost)


init = tf.global_variables_initializer()

with tf.Session() as sess :
  sess.run(init)
  print(w.eval())
  for i in range(100):
    sess.run(train,feed_dict = {
        X : radii,
        Y : distance
    })
  print(w.eval())
```

While training, after just a few iterations, we will see that the network converged and the scalar **w** we are training is about `6.2831855` which is the desired value




