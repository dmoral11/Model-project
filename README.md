# Model-project
# -*- coding: utf-8 -*-
"""
Created on Sun Apr 30 12:56:32 2017

@author: Daniela Morales C
"""

#Proyecto Modelacion


import numpy as np

print('Todos los valores deben estar en kg, cm')
m = int(input("Ingrese número de momentos: "))
M = np.zeros((m,2)) #Matriz que acomula los momentos y las distancias

for i in range(m):
    count=0
    for j in range(2):
        count+=1
        if count==2:
          M[i,j] = float(input("Distancia: ")) # Llenando la matriz  
        else:
            M[i,j] = float(input("Momento: ")) # Llenando la matriz
 
print("Matriz digitada: ")
print(M)

#Datos adicionales
h = float(input("ingrese valor de la altura: "))
e = float(input("ingrese valor del recubrimiento: "))
b = float(input("ingrese valor de la base: "))
fc = float(input("ingrese valor f'c: "))
fy = float(input("ingrese valor fy: "))
dp = float(input("ingrese valor d\': "))

fi=0.9

d=float(h)-float(e)
em=fy/(0.85*fc)
print('em es: ', round(em,2))

R = np.zeros(int(m))

j=0
for j in range(m): 
   R[j] = (M[j,0]*(10**5))/(fi*b*(d**2)) #Calculo de "R"

print("Verctor R: ")
print(R)   

if fc <= 280: #Calculo de beta
 bi=0.85
elif fc>=560:
 bi=0.65
else:
    bi=0.85-(fc-280)*(0.05/70)

p1=14/fy   #Cuantia minima
p2=(0.8*(fc**0.5))/fy
if p1>p2:
    pmin=p1
else:
    pmin=p2
print("Pmin: ") 
print(pmin)

cd=0.003/(0.005+0.003) #Cuantia maxima
pmax=(bi*cd)/em
print("Pmax: ") 
print(pmax)

  
Pdi = np.zeros(int(m))
P = np.zeros(int(m))
n=0
for n in range(m):
   DM=0
   As1=0
   As2=0
   Rmax=0
   Pdi[n] = (1/em)*(1-((1-((2*em*R[n])/fy))**0.5)) #Calculo de la cuantia de diseño
   if Pdi[n]<pmin:
    P[n]=pmin
   elif Pdi[n]>pmax:
     Rmax=float(pmax*fy*(1-((pmax*em)/2)))
     DM = float((M[n,0])-(Rmax*fi*b*(d**2))/(10**5))
   
     As1=float(pmax*b*d)
     c=float((As1*fy)/(0.85*fc*b*bi))
     if c<d:
         esp=(0.003*(c-dp))/c
         es=(0.003*(d-c))/c
     else:
         print('Error')
         
     if esp>0.0020 and es>=0.005:
         As2=float((DM*(10**5))/(fi*fy*(d-dp)))
         As=As1+As2
         P[n]=float(As/(b*(d**2)))
     else:
         print('Error')
   else:
         P[n]=Pdi[n]


print("Verctor Pdi: ")
print(Pdi) 

print("Verctor P: ")
print(P) 

A = np.zeros(int(m))
for j in range(m):
    A[j] = P[j]*b*d
     
print("Verctor A: ")
print(A) 

