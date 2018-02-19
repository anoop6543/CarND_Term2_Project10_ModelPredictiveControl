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
```
N = 12(little higher than Project Overview video)
dt = 0.1
T = 1 second
```

# My Plan to tuning as per the suggestions mentioned in the Project overview video and slack channels.

1. Tweak/change the N value between 5 to 15 while keepng the dt and T constant.
2. change the value between 0.05 and 0.15.
3. value of T between 0.5 seconds to 2 seconds.

Ultimately the results obtained would be used for the best Model Predictive Controller(Tuned).

Latency computation is used from inputs from various forum discussuions. Latency was basically considered as a time change in the state equation
