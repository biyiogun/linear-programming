# linear-programming
Explains the linear programming examples
#linear programming: Profit maximisation

#max: 8x1 + 5x2 + 10x3
#      s.t.
 #      2x1+3x2x3<=400  production time constraint (hrs)
    #    x1+x3<= 150    production component constraint
    #   2x1+4x3<=200    input constraint
    #       x2<=50      maximum quantity producable (quota)
dec= [8,5,10]  #dec=decision variables
constr= [[2,3,1],[1,0,1],[2,0,4],[1]]  #constr= constraints
RHS=[400,150,200,50]  #solution quantities
equality= [GRB.LESS_EQUAL, GRB.LESS_EQUAL, GRB.LESS_EQUAL, GRB.LESS_EQUAL]  #inequality signs
#create model
LP= Model('LP')

#add variables
x=np.array([None for j in range(3)])
for k in range(3):
    x[k]=LP.addVar(name= 'x%d' %k)
LP.update()

#set objective
LP.setObjective(np.dot(dec, x), GRB.MAXIMIZE)
#add constraints
for i in range(3):
    LP.addConstr(np.dot(constr[i], x), equality[i], RHS[i])
LP.update()

print('LP.status:', LP.status)

LP.write('Quest.lp')
LP.optimize()

for i in range(3):
    x[i]= LP.getVars()[i].x
    print(x[i])
print('Obj: %g' % LP.objVal)





#transportation problem
from gurobipy import * 
import numpy as np
n = 12
cost =np.array([20,30,110,70,10,0,60,10,50,80,150,90]) #transportation costs
#create model
m=Model('ORexam')
#add variable
x=np.array([None for j in range(n)])
for k in range(12):
    x[k]= m.addVar(name = 'x%d' %k)
m.update()
#Set objective
m.setObjective(np.dot(cost,x), GRB.MINIMIZE)
#add constraints
m.addConstr(x[0]+x[1]+x[2]+x[3]<=6000) #ondo factory constraint
m.addConstr(x[4]+x[5]+x[6]+x[7]<=1000) #osun factory constraint
m.addConstr(x[8]+x[9]+x[10]+x[11]<=10000) #oyo factory constraint
m.addConstr(x[0]+x[4]+x[8]>=7000) #abuja depot constraint
m.addConstr(x[1]+x[5]+x[9]>=5000) #lagos depot constraint
m.addConstr(x[2]+x[6]+x[10]>=3000) #kano depot constraint
m.addConstr(x[3]+x[7]+x[11]>=2000) #enugu depot constraint

#update Linear Program
m.update()
print('m.status:', m.status)
m.write('Exam answer.lp')

#optimise model
m.optimize()
for i in m.getVars():
    print('%s %g' % (i.varName, i.x))
print('Obj: %g' % m.objVal)
