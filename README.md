# Zombie-apocalipse-
#!/usr/bin/env python3
# -*- coding: utf-8 -*-


import time
import matplotlib.pyplot as plt
import random
import matplotlib
from colorama import Fore, Back, Style


'''Sus = Susceptible
   Inj = Injured
   Inf = Infected'''


trapx =[]
trapy =[]

'''If user tries to defy me then boundary is set to 6'''

boundary = [0,0]
boundary[0]=int("{}".format(input("boundary x has to be greater than 5 :")))
if boundary[0] <= 5:
    boundary[0] = 6
    print("bountday x is {}".format(6))

boundary[1]= int("{}".format(input("boundary y has to be greater than 5 :")))
if boundary[1] <= 5:
    boundary[1] = 6
    print("bountday y is {}".format(6))
    
if boundary[0]>boundary[1]:
    highest = boundary[0]
    lowest = boundary[1]
else:
    highest =boundary[1]
    lowest =boundary[0]
'''the number of traps is equal to either boundary[0] or  boundary[1], which ever is higher'''
for i in range(highest):
    trapx.append(random.randint(0,highest))
    trapy.append(random.randint(0,lowest))

trapLocations = [0]*len(trapx)

for i in range(len(trapx)):
    trapLocations[i]= trapx[i],trapy[i]
                


class Person():
  
    def __init__(self):
        
         
        self.age = random.randint(18, 70)
        #sets the x and y location of every one to random numbers between the boundaries
        self.location = [
            random.randrange(0, boundary[0]),
            random.randrange(0, boundary[1])]  
        self.health = 10
        self.onTrap = False
        
 
    def takeDamage(self, damage):
        self.health -= damage
        
    '''Trapped() compares the location of the traps and the location of each person
       If they land on a trap and are not Susceptible, the lose 50 health
       If they are susceptible then they have a chance of dodging the trap'''
    
    def trapped(self):    
        for j in trapLocations: 
                if self.location[0] == j[0] and self.location[1] == j[1]:
                    self.onTrap = True
                    if self.__class__ == Sus:  
                        pass 
                    else: 
                        self.health -= 50
                        
    '''move(self) basically moves everyone to a new location determined by their 
       health and age and checks if they have crossed the boundary of the terrain 
       If they have then their new location becomes the boundary of the terrain'''

    def move(self): 
        if self.age <= 45 and self.health >=50 :       
        
            self.location[0] += random.randint(-5,5)
            
            if self.location[0] > boundary[0]:   
                self.location[0] = boundary[0] 
            elif self.location[0] < 0: 
                self.location[0] =0 
          
        
            self.location[1] += random.randint(-5,5) 
          
            if self.location[1] >boundary[1]:   
                self.location[1] = boundary[1] 
            elif self.location[1] < 0: 
                self.location[1] =0 
          
        elif self.age <= 45 and self.health <= 50:  
            
            self.location[0] += random.randint(-4,4)             
          
            if self.location[0] > boundary[0]:   
                self.location[0] = boundary[0] 
            elif self.location[0] < 0: 
                self.location[0] =0 
        
            self.location[1] += random.randint(-4,4) 
        
            if self.location[1] >boundary[1]:   
                self.location[1] = boundary[1] 
            elif self.location[1] <0:
                self.location[1] =0 
              
        elif self.age >= 45 and self.health >= 50: 
             
            self.location[0] += random.randint(-3,3)  
          
            if self.location[0] > boundary[0]:   
                self.location[0] = boundary[0] 
            elif self.location[0] < 0:
                self.location[0] =0 
        
            self.location[1] += random.randint(-3,3) 
        
            if self.location[1] >boundary[1]:   
                self.location[1] = boundary[1] 
            elif self.location[1] <0:
                self.location[1] =0 
          
          
        elif self.age >= 45 and self.health <= 50: 
            
            self.location[0] += random.randint(-2,2)  
          
            if self.location[0] > boundary[0]:   
                self.location[0] = boundary[0] 
            elif self.location[0] < 0:
                self.location[0] =0 
          
            self.location[1] += random.randint(-2,2) 
          
            if self.location[1] > boundary[1]:   
                self.location[1] = boundary[1] 
            elif self.location[1] <0:
                self.location[1] =0 
        
class Sus(Person): 
    def __init__(self): 
        super().__init__()      # assigns all Person() attributes and methods to Sus() class     
        self.health = 100      #Overide the health variable by setting it to 100                  
    def getLocation(self):
        return self.location
    def checkForTrap(self):
        return self.onTrap
    
    def DodgeObstacles(self):       
        y = random.randint(0,12)             
        if y % 2 == 0:  
            return True 
        else:                                 
            return False 
    def susCheckAttack():
        return Inf.Attack() 

class Inj(Person): 
    def __init__(self): 
        super().__init__()  
        self.health= 50 
    def getLocation(self):
        return self.location 
    def injCheckAttack():
        return Inf.Attack() 
    def rest(self):               
        y = random.randint(0,11)          
        if y == 6 : 
            if self.health<=60:
                self.health += 20   
        
class Inf(Person): 
    def __init__(self): 
        super().__init__()  
        self.health = 100               
  
    def Elixir(self): 
        y = random.randint(0,12)    
        if y  == 6:  
            if self.health <= 70: 
                self.health += 30 
    def Attack(self):  
        if Sus().getLocation() == self.location:  
            y = random.randint(0,10)
            if y % 7 == 0:
                return True
            
        elif Inj().getLocation() == self.location: 
            y = random.randint(0,10)
            if y == 7:
                return True
          
             #The person is then added to the array of Infected 

'''limit determines how many people for each class you are allowed to set 
   That is determined by the X by Y boundary. By multiplying both x and y boundary
   you know how many possible positions can be plotted on the plane
   By subtracting highest, multiplying 80 and dividing by 100, you know 80% of the remaining 
   positions excluding the traps. Now you simply divide by 3 for Inf, Sus,Inj'''
   
limit = ((((boundary[0]*boundary[1])-highest)*80)//100)//3

numSus =int("{}".format(input("input number of Sus must be less than {} :".format(limit))))
if numSus > limit:  #sets numSus to limit if the inputed number is above limit
    numSus = limit
    print("number of Sus is {}".format(limit))
numInf = int("{}".format(input("input number of Inf must be less than {} :".format(limit))))
if numInf > limit:
    numInf = limit
    print("number of Inf is {}".format(limit))
numInj = int("{}".format(input("input number of Inj must be less than {} :".format(limit))))
if numInj > limit:
    numInj = limit
    print("number of Inj is {}".format(limit))

CountSus =[] 
CountInf= [] 
CountInj= [] 
susx = []
susy =[]
infx =[]
infy =[]
injx=[]
injy =[]

def main(): # runs the whole simulation

    for i in range(numSus):  #creates objects of class Sus() and adds it to CountSus[]
        CountSus.append(Sus())
        susx.append(CountSus[i].location[0])
        susy.append(CountSus[i].location[1])
        
    for i in range(numInf):
        CountInf.append(Inf())
        infx.append(CountInf[i].location[0])
        infy.append(CountInf[i].location[1])
    
    for i in range(numInj):
        CountInj.append(Inj())
        injx.append(CountInj[i].location[0])
        injy.append(CountInj[i].location[1])
  
   
   
   
    CountRem = 0
    start =time.time()
    graphSus = []
    graphInf =[]
    graphInj =[]
    while  len(CountSus)+len(CountInj)>0 :
        
        del susx[:]      #deletes all elements in susx
        del susy[:]
        del injx[:]
        del injy[:]
        del infx[:]
        del infy[:]
        ''' for every person in its designated CountClass checks for traps, moves to a new
            location... If their health gets below 0 then they are removed'''
        for per in CountSus:          
            per.trapped()
            per.move()
            if per.checkForTrap() ==True:
                if per.DodgeObstacles() == True:
                    pass
                else:
                    per.takeDamage(50)
            if per.health <= 0 or per.susCheckAttack == True:
                CountSus.remove(per)
                CountRem += 1
            else:     # gets all the survivors of the first round location
                susx.append(per.location[0]) 
                susy.append(per.location[1])
        
        for per in CountInf:                   
            per.trapped()
            per.move()
           
            per.Elixir()
            if per.Attack() == True:               
                CountInf.append(Inf())

            if per.health <=0 :
                CountInf.remove(per)
                CountRem +=1 
            else:     # gets all the survivors of the first round location
                infx.append(per.location[0])
                infy.append(per.location[1])
                
        for per in CountInj:                   
            per.trapped()
            if per.rest() == True:
                pass
            else:
                per.move()
            
            if per.health <=0 or per.injCheckAttack == True: 
                CountRem +=1
                
                CountInj.remove(per)
                
            else:    # gets all the survivors of the first round location
                injx.append(per.location[0])
                injy.append(per.location[1])
    
        '''records number of survivors for each round '''
     
        graphSus.append(len(CountSus))
        graphInf.append(len(CountInf))
        graphInj.append(len(CountInj))
        
        '''plots every survivor on the scatter plot for each round''' 
        
        plt.scatter(susx,susy,c='b',marker='.',label='sus') 
        plt.scatter(injx,injy,c='r',marker='.',label='inj')
        plt.scatter(infx,infy,c='y',marker='.',label='inf')
        plt.scatter(trapx,trapy,c='k',marker='x',label ='traps')
        ax = plt.subplot(111)
        ax.legend(loc='upper center', bbox_to_anchor=(0.5, -0.05),
          fancybox=True, shadow=True, ncol=5)
        plt.show()
        
        '''bc it lags when it is iterating, the pause time is set determining
           on the amount of ppl left'''
        if len(CountSus)+len(CountInj)+len(CountInf) <= 10:
            pass
        elif len(CountSus)+len(CountInj)+len(CountInf) <= 20:
            time.sleep(.01)
        elif len(CountSus)+len(CountInj)+len(CountInf) <= 100:
            time.sleep(.1)
        elif len(CountSus)+len(CountInj)+len(CountInf) >100:
            time.sleep(.2)
         #pauses code for x amount of time before continuing 
       
        
        #input("Press Enter to continue...")
         
        print(".",end=" ") # records module progression by printing a dot after every round
        
        
    stop=time.time()  # stops the timer
    time1 =round(stop-start)
    print("\nIt took {} seconds to complete the loop\n".format(time1))
    print("{} Susceptibles left\n".format(len(CountSus)))
    print("{} Injured left\n".format(len(CountInj)))
    print("{} Infected left".format(len(CountInf)))
    print("\n{} where removed".format(CountRem))
    
    ''' graphs the recorded survivors from lines 287 - 289'''
    
    plot = matplotlib.pyplot
    plot.plot(graphSus,label ="Susceptable")
    plot.plot(graphInf,label ="Infected")
    plot.plot(graphInj,label ="Injured")
    time.sleep(3)  # waits x seconds before showing the graph
    plot.legend()
    plot.show()
    

Main=main() 


