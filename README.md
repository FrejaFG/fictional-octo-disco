# -*- coding: utf-8 -*-
"""
Created on Thu Sep 26 13:34:59 2019

@author: Freja
"""

import numpy as np
import matplotlib.pyplot as plt
from scipy import optimize
#from scipy.optimize import curve_fit #optimering af fit.

#importer data, delimiter=, fortæller python det er kommasepereret. 
data = np.genfromtxt("lydUnderVand.csv",delimiter=";",skip_header=1)
#data=np.char.replace(np.genfromtxt("lydUnderVand.csv",delimiter=";",skip_header=1,dtype="str"),"."),astype=("allfloat")

#nu ipmorteres de tre kolonner :=hele rækken, efterfulgt at nr. 
laengde=data[:,0]
tid=data[:,1]
usikkerhed=data[:,2]

#en linear funk. og dens parametre defineres
def linFunc(x,a,b):
    y = a*x+b
    return y

#Return statement is used to exit the funk. and go back from where it was called. 
#Return indeholder det evaluerede udtryk. 

#vi kan fitte nu, på en funktion med x og y-værdi.
#usikkerheden plottes mes y-værdien.
#absolute_sigma=true - tager højde for usikkerheden. 
par, cov=optimize.curve_fit(linFunc,laengde,tid,sigma=usikkerhed, absolute_sigma="true")
#par et et array np.array=([a,b]), ab er værdierne for fittet. 
#cov=2d array med usikkerhed
print(par[0])

#plot. luk eventuelt åbne plots
plt.close("all")
plt.rc("font",size=15) #al skift str. 15

#lav nogle x-værdier mellem 0 og 50s. 
xs=np.linspace(0,500,100000)
#og y-værdier. 

y=np.asarray(linFunc(xs,par[0],par[1]))
#plot og navngiv 
plt.plot(xs,y,label="fit")
#plot data med errorbars
plt.errorbar(laengde,tid, yerr=usikkerhed, fmt="o",capsize=8, label="lyd under vand")
#tilføj aksebetegnesler.
plt.xlabel("længden fra udsendelse [m]")
plt.ylabel("tid fra udsendelse [s]")

plt.tight_layout()#intet klippes fra grafen.
plt.savefig("fit af lydens hastighed.png",dpi=300)
