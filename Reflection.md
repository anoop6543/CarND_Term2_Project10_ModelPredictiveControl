## Reflection: ##

**Credits:** The project is completed with help of Udacity's Project Overview Video by Aaron brown and quizzes provided along with classroom material.

**Disclaimer:** I couldn't get the simualtor launched on my Operating system. I was able to get only to the level succesfully building and cmake of the code. I've been getting the error the process of ***make***.

### Model Predictive Controller 

**State equations**
```
x' = x + v*cos(psi)*dt
y' = y + v*sin(psi)*dt
psi' = psi + v/Lf*delta*dt
v' = v + a*dt
```

x', y', psi', v' -  Value of the next state based on current state
x, y, psi, v - These are actuated by the steering parameter and throttle control (`delta` and `a`- acceleration\retardation).

**Error Equation**
```
cte' = cte + vt*sin(e_psi)*dt
cte' = f(x_t) - y_t + vt*sin(e_psi)*dt
```

```
e_psi' = e_psi + v/Lf * delta * dt
psi_des = arctan(f'(x_t)) i.e. tangential angle of polynomial f evaluated at x_t.
e_psi = psi - arctan(f'(x_t))
```

Horizon(T), Number of Timesteps(N), Timesteps duration(dt) values are approximately taken from Project overview video as mentioned below.

**T** - T can be 1 to 2 seconds. Anything more , the environment will affect the prediction and the future predictions doesn't match the requirement. Environment change is also affected by car speed. At lower speeds, T can be increased a bit more and environment change is slower. At higher speeds, the higher value of T doesn't help in predicting future states because the state changes faster than the rate at which we estimate the values of next state. So for higher values of T, It's not meaningful in computing future states after the state change happend.(i.e state changes happens faster than prediction)

**N** - Number of time steps to future depending on the present state. length of control vector is tuned based on Number of TimeSteps(N). If the value of N is higher, more number of timesteps will need to be computed and it delays the process and in contrast lower value of N will tend to have less number of predictions into future state. An optimized value can be tuned/selected for N. 

**dt** - It is the length of each timestep. more dt increases the accuracy. dt is proportional to T.

Different scenarios : - 
case 1)  N=5, dt=0.1, T=0.5 seconds 
case 2) N=5, dt=0.05, T=0.25 seconds
case 3) N=15, dt=0.05, T=0.75 seconds
case 4) N=15, dt=0.1, T=1.5 seconds

From case 1 to case 2, the N value didn't change but dt value changed to half, so here accuracy increase but since T is lower the time was not enough to predict the future values.
Another ex :- 
From case 2 to case 3, N value increased and T value also increased. Increase in value of N leads to more computation time. low dt value still gives a higher accuracy. But due to heavy computation and increase in trajectory length, the car tends to fluctuate between right and left. It adds latency in steering actuation. The car tends to lose the track of road.

So now a decrease in (N)Number of time steps is necessary and decrease in T is also necessary to make sure the car drives in the center. accuracy can be adjusted based on requirement. So N = 10-12,dt = 0.1 and T = 1-1.2 seconds helps in getting better results.

```
N = 10(Value mentioned in the Project Overview video)
dt = 0.1
T = 1 second
```
# Waypoint pre processing
The waypoint preprocessing is done by transforming the values to vehicles perception. 
The process is then simplified and fit to a polynomial to the waypoints. 
Vehicle's x and y coordinates are at the origin (0, 0).
Orientation angle will be zero.

#Latency 
Latency computation is used from inputs from various forum discussuions. Latency was basically considered as a time change in the state equation
```
double latency = 0.1;
px = px + v*cos(psi)*latency;
py = py + v*sin(psi)*latency;
psi = psi - v*steer_value/Lf*latency;
v = v + throttle_value*latency;
```
From the above set of equations, it's observed that dt is replaced by latency and remaining is similar to the state equations of the MPC. In this case, the speed can be increased to higher speeds ~(70-80) kmph as mentioned in 2nd review of submission due to the optimization technique.




# My Plan to tuning as per the suggestions mentioned in the Project overview video and slack channels.

1. Tweak/change the N value between 5 to 15 while keepng the dt and T constant.
2. change the value between 0.05 and 0.15.
3. value of T between 0.5 seconds to 2 seconds.

Ultimately the results obtained would be used for the best Model Predictive Controller(Tuned).


