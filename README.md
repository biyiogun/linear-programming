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
