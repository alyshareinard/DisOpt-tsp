#!/usr/bin/python
# -*- coding: utf-8 -*-

import math
from collections import namedtuple
from operator import itemgetter, attrgetter
import numpy 
import matplotlib.pyplot as plt
plt.ion()

Point = namedtuple("Point", ['ind', 'x', 'y'])

def length(point1, point2):
    return math.sqrt((point1.x - point2.x)**2 + (point1.y - point2.y)**2)

def length3(point1, point2, point3):
    return length(point1, point2)+length(point2, point3)

def length4(point1, point2, point3, point4):
    return length(point1, point2)+length(point2, point3)+length(point3, point4)

def tour_length(points, solution):
    obj = length(points[solution[-1]], points[solution[0]])
    for index in range(0, len(solution)-1):
       obj += length(points[solution[index]], points[solution[index+1]])
#        print "+", length(points[solution[index]], points[solution[index+1]]);
    return obj

def plot_tour(points, solution):
    plt.clf()
    pointsx=[]
    pointsy=[]
    for i in solution:
#        print "i", i
        pointsx.append(points[i].x)
        pointsy.append(points[i].y)
    plt.plot(pointsx, pointsy, 'yo-')
    plt.draw()
#    raw_input()

def islineintersect(line1, line2):
    i1 = [min(line1[0][0], line1[1][0]), max(line1[0][0], line1[1][0])]
    i2 = [min(line2[0][0], line2[1][0]), max(line2[0][0], line2[1][0])]
    ia = [max(i1[0], i2[0]), min(i1[1], i2[1])]
    if max(line1[0][0], line1[1][0]) < min(line2[0][0], line2[1][0]):
        return False
    m1=0
    m2=0
    if line1[1][0]!=line1[0][0]: 
        m1 = (line1[1][1] - line1[0][1]) * 1. / (line1[1][0] - line1[0][0]) * 1.
    if line2[1][0]!=line2[0][0]: 
        m2 = (line2[1][1] - line2[0][1]) * 1. / (line2[1][0] - line2[0][0]) * 1.
    if m1 == m2:
        return False
    b1 = line1[0][1] - m1 * line1[0][0]
    b2 = line2[0][1] - m2 * line2[0][0]
    x1 = (b2 - b1) / (m1 - m2)
    if (x1 < max(i1[0], i2[0])) or (x1 > min(i1[1], i2[1])):
        return False
    return True

def solve_it(input_data):
    # Modify this code to run your optimization algorithm

    # parse the input
    lines = input_data.split('\n')

    nodeCount = int(lines[0])

    points = []
    for i in range(1, nodeCount+1):
        line = lines[i]
        parts = line.split()
        points.append(Point(i-1, float(parts[0]), float(parts[1])))

    #let's order the points by x coordinate, then by y coordinate
#    points_sorted=sorted(points, key=attrgetter('x', 'y'))
    points_sorted=sorted(points, key=attrgetter('y', 'x'))
    best_solution=[]
#    print "points", points
    for i in range(nodeCount):
#        print "points_sorted.index", i, points_sorted[i].ind
        best_solution.append(points_sorted[i].ind)
    node_max=nodeCount-1
    count_max=2#0
    
    for num in range(10, node_max):
        left=range(nodeCount)
        print "num", num
        min_distances=[num] 
        left.remove(num)
        old_node=0
        distance_old=100000.0
        for i in range(1, nodeCount):
            for j in left:
                distance=length(points[old_node], points[j])
#                print "old node", old_node
#                print "distance", distance
#                print "distance old", distance_old
                if distance<distance_old:
                    distance_old=distance
                    next_node=j
#                    print "next node", next_node
#                        print "setting next node to ", next_node
            min_distances.append(next_node)
            print "adding", next_node
            left.remove(next_node)
#            print "min_dist", min_distances
            old_node=next_node
            next_node=-1
            distance_old=100000.0

#    print "min_distance", min_distances
#    print "len min distance", len(min_distances)

#    distance_array=numpy.zeros((nodeCount, nodeCount))
#    for i in range(0, nodeCount-1):
#        for j in range(0, nodeCount-1):
#            distance_array[i,j]=length(points[i], points[j])
#            if i==j:
#                distance_array[i,j]=9999
#    print distance_array

#    for i in range(0, nodeCount-1):
#        temp=min(distance_array[:,i])
#        print "temp", temp
#        val, idx=min((val, idx) for (idx, val) in enumerate(distance_array[:,i]))
#        print "val, idx", val, idx
#        min_distances.append(idx)
#        print "min_distnace", min_distances
#        #now black out the other options
#        distance_array[idx,:]=9999
#        print distance_array
#    print "min_ditances", min_distances



#let's leave them in the original order 
#    solution=range(nodeCount)
#    solution=best_solution
        solution=min_distances
    # calculate the length of the tour 
        plot_tour(points, solution)
        tour_total=tour_length(points, solution)
        print "tour total", tour_total
        this_tour=tour_total-1
        count=0
        change_first=1
#    now step through and try swapping nodes
        while(this_tour!=tour_total and count<count_max):# or change_first==0):
#    for count in range(0, 20):
            if this_tour == tour_total:
                temp=solution.pop(0)
                solution.append(temp)
                change_first=1


            tour_total=this_tour
            count+=1
            print "time through", count

        #check for lines that cross
            for index1 in range(0, nodeCount-1):
                for index2 in range(index1+2, nodeCount-1):
                    if index1!=index2 and solution[index1+1]!=solution[index2] and solution[index1]!=solution[index2+1]:
                       #print "index1", points[solution[index1+1]].x
                       #print "index2", points[solution[index2+1]].x
                        if islineintersect([(points[solution[index1]].x, points[solution[index1]].y), (points[solution[index1+1]].x, points[solution[index1+1]].y)], [(points[solution[index2]].x, points[solution[index2]].y), (points[solution[index2+1]].x, points[solution[index2+1]].y)]):
#                        a=0
#                            print "found intersection"
#                        print solution
                            temp=[]
                            for i in range(index1, index2+1):
                                temp.append(solution[i])
#                        print temp
                            for i in range(index2, index1-1, -1):
#                            print "i", i
                                solution[i]=temp.pop(0)
#                        print solution

#                        print solution[index1], solution[index1+1], solution[index2], solution[index2+1]
#                        print points[solution[index1]].x, points[solution[index1+1]].x, points[solution[index2]].x, points[solution[index2+1]].x
#                        print points[solution[index1]].y, points[solution[index1+1]].y, points[solution[index2]].y, points[solution[index2+1]].y
                            temp=solution[index1]
                            solution[index1]=solution[index2]
                            solution[index2]=temp



            for index in range(-1, nodeCount-1):
                current_dist=length3(points[solution[index-1]], points[solution[index]], points[solution[index+1]])

        #now, which node should be between these to minimize the distance -- we loop through all the other nodes
                for trial in range(index+1, nodeCount-1):
                    new_dist1=length3(points[solution[index-1]], points[solution[trial]], points[solution[index+1]])
#                    print "trial", trial
                    current_dist2=length3(points[solution[trial-1]], points[solution[trial]], points[solution[trial+1]])
                    new_dist2=length3(points[solution[trial-1]], points[solution[index]], points[solution[trial+1]])
                    if trial == index+1: 
                    #special case
#                    print "special case 1"
                        new_dist1=length3(points[solution[index-1]], points[solution[trial]], points[solution[index]])
                        new_dist2=length3(points[solution[trial]], points[solution[index]], points[solution[trial+1]])
#                print "index", index
#                print "trial", trial
#                print "nodeCount", nodeCount
                    if index==-1 and trial==nodeCount-2: 
                    #special case
 #                       print "special case 2"
                        new_dist1=length3(points[solution[index]], points[solution[trial]], points[solution[index+1]])
                        new_dist2=length3(points[solution[trial-1]], points[solution[index]], points[solution[trial]])

                    new_diff=current_dist+current_dist2-new_dist1-new_dist2
                #print "new_diff", new_diff
                    if new_diff>0:
#                        print "swapping ", index, "and ", trial
#                        print "shrinking by", new_diff
 #                   print "current_dist", current_dist
 #                   print "current_dist2", current_dist2
 #                   print "new_dist1", new_dist1
 #                   print "new_dist2", new_dist2

#                        print "making new solution"
                        temp=solution[index]
                        solution[index]=solution[trial]
                        solution[trial]=temp
                        current_dist=length3(points[solution[index-1]], points[solution[index]], points[solution[index+1]])
#                    print "new current dist", current_dist
        #also check for doubles
            for index in range(-1, nodeCount-2):
                current_dist=length4(points[solution[index-1]], points[solution[index]], points[solution[index+1]], points[solution[index+2]])

        #now, which node should be between these to minimize the distance -- we loop through all the other nodes
                for trial in range(index+3, nodeCount-2):
                    new_dist1=length4(points[solution[index-1]], points[solution[trial]], points[solution[trial+1]], points[solution[index+2]])
#                    print "trial", trial
                    current_dist2=length4(points[solution[trial-1]], points[solution[trial]], points[solution[trial+1]], points[solution[trial+2]])
                    new_dist2=length4(points[solution[trial-1]], points[solution[index]], points[solution[index+1]], points[solution[trial+2]])
                    new_diff=current_dist+current_dist2-new_dist1-new_dist2
                #print "new_diff", new_diff
                    if new_diff>0:
#                        print "found a double"
#                        print "swapping ", index, "and", index+1, "with", trial, "and", trial+1
#                        print "shrinking by", new_diff
#                    print "current_dist", current_dist
#                    print "current_dist2", current_dist2
#                    print "new_dist1", new_dist1
#                    print "new_dist2", new_dist2

                        temp=solution[index]
                        solution[index]=solution[trial]
                        solution[trial]=temp
                        temp=solution[index+1]
                        solution[index+1]=solution[trial+1]
                        solution[trial+1]=temp
                        current_dist=length4(points[solution[index-1]], points[solution[index]], points[solution[index+1]], points[solution[index+2]])
#                    print "new current_dist", current_dist
 
            this_tour=tour_length(points, solution)
            print "this_tour", this_tour
            plot_tour(points, solution)
 
        if tour_length(points, solution) < tour_length(points, best_solution):
            print "found a better solution!"
            best_solution=solution
            
    print "finished with loop"
    print "final length", tour_length(points, best_solution)
    raw_input
    solution=best_solution
        

        

    #now find the smallest values
    raw_input()
    plt.show()
#    best_solution=[]
#    print "points", points
#    for i in range(nodeCount):
#        print "points_sorted.index", i, points_sorted[i].ind
#        best_solution.append(points[i].ind)
    
    # build a trivial solution
    # visit the nodes in the order they appear in the file
#    solution = best_solution
#    solution=range(nodeCount)
#    solution=best_solution
#    solution=min_distances

    # calculate the length of the tour
    obj = length(points[solution[-1]], points[solution[0]])
    print "obj", obj
    for index in range(0, nodeCount-1):
        obj += length(points[solution[index]], points[solution[index+1]])
        print "+", length(points[solution[index]], points[solution[index+1]])

    # prepare the solution in the specified output format
    output_data = str(obj) + ' ' + str(0) + '\n'
    output_data += ' '.join(map(str, solution))

    return output_data


import sys

if __name__ == '__main__':
    if len(sys.argv) > 1:
        file_location = sys.argv[1].strip()
        input_data_file = open(file_location, 'r')
        input_data = ''.join(input_data_file.readlines())
        input_data_file.close()
        print solve_it(input_data)
    else:
        print 'This test requires an input file.  Please select one from the data directory. (i.e. python solver.py ./data/tsp_51_1)'

