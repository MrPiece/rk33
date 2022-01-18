# Импорт библиотек
import matplotlib.pyplot as plt
import numpy as np
from matplotlib.pyplot import figure

# Логистическая функция
def logistic_eq(r,x):
    return r*x*(1-x)

# Логистическая функция без запаздывания(для отрисовки графика)
def logistic_equation_orbit(x_init, r, n_iter):
    print('Orbit for x with init value={0}, '
          'r={1}, '
          'iterations={2}'.format(x_init, r, n_iter))
    X_t=[]
    T=[]
    t=0
    x_curr = x_init

    for i in range(n_iter):
        
        X_t.append(x_curr)
        T.append(t)
        
        t+=1
        x_curr = logistic_eq(r, x_curr);

    plt.figure(figsize=(8, 8))
    plt.plot(T, X_t, color='blue')
    plt.ylim(0, 1)
    plt.xlim(0, T[-1])
    plt.xlabel('t')
    plt.ylabel('X(t)')
    plt.show()
    
# Логистическая функция с запаздыванием
def logistic_eq_with_hysteresis(r, x_curr, x_prev):
    return r*x_curr*(1-x_prev)
    
# Логистическая функция с запаздыванием (для отрисовки графика)
def logistic_equation_orbit_with_hysteresis(x_prev, x_curr, r, n_iter):
    print('Orbit for x_first_given={0}, '
          'x_second_given={1}, '
          'r={2}, '
          'iterations={3} '.format(x_prev, x_curr, r, n_iter))
    
    X_t=[x_prev]
    T=[0]
    t=1
    
    for i in range(n_iter):
        
        X_t.append(x_curr)
        T.append(t)
        
        F_curr_updated = logistic_eq_with_hysteresis(r, x_curr, x_prev)
        x_prev=x_curr
        x_curr=F_curr_updated
        t += 1
        
    plt.figure(figsize=(8, 8))
    plt.ylim(0, 1)
    plt.xlim(0, T[-1])
    
    plt.xlabel(f'n={n_iter}\nr={r}')
    plt.ylabel('X(t)')
    
    plt.plot(T, X_t, color='blue')
    plt.show()
    
def bifurcation_diagram(seed, n_skip, n_iter, step=0.0001, r_min=0):
    print("Starting with x0 seed {0}, skip plotting first {1} iterations, then plot next {2} iterations.".format(seed, n_skip, n_iter));
    
    R = []
    X = []
    
    r_range = np.linspace(r_min, 4, int(1/step))

    for r in r_range:
        x = seed;

        for i in range(n_iter+n_skip+1):
            if i >= n_skip:
                R.append(r)
                X.append(x)
                
            x = logistic_eq(r,x);
            
    plt.figure(figsize=(8, 8)) 
    plt.plot(R, X, ls='', marker=',')
    plt.ylim(0, 1)
    plt.xlim(r_min, 4)
    plt.xlabel('r')
    plt.ylabel('X')
    plt.show()
image.png

# Сценарий, при 0 < r < 2
# r = 1.7
# кол-во итераций 200
# x(0) = 0.85
# x(1) = 0.30
logistic_equation_orbit_with_hysteresis(0.85, 0.30, 1.7, 200)

# Анализ графика #
#Здесь можно увидеть колебания, которые стремятся к точке покоя
Orbit for x_first_given=0.85, x_second_given=0.3, r=1.7, iterations=200 

# Сценарий, при r > 2 
# r = 2.1
# кол-во итераций 200
# x(0) = 0.85
# x(1) = 0.25
logistic_equation_orbit_with_hysteresis(0.85, 0.25, 2.1, 200)

# Анализ графика #
#Здесь можно увидеть цикличные колебания.
Orbit for x_first_given=0.85, x_second_given=0.25, r=2.1, iterations=200 

# Сценарий, при r > 2.27 
# r = 2.5
# кол-во итераций 200
# x(0) = 0.85
# x(1) = 0.25
logistic_equation_orbit_with_hysteresis(0.85, 0.25, 2.5, 200)

# Анализ графика #
#Здесь можно увидеть угасающая диффузия (но не хаос)
Orbit for x_first_given=0.85, x_second_given=0.25, r=2.5, iterations=200 

