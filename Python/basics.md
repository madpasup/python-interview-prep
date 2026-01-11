## Python Interview Questions
numbers = [1, 2, 2, 3, 1, 4, 2, 3, 5, 1]
res_dict ={num:numbers.count(num) for num in numbers}
print(res_dict)


students = [{"Alice":{"Math":85,"Science":90,"English":78}},
          {"Bob":{"Math":72,"Science":88,"English":80}},
          {"Charlie":{"Math":95,"Science":92,"English":85}}]
# Write a python program to obtain output: {'Alice':253,'Bob':240,'Charlie':272}
result = {}
# for students,m
Charlie_total_marks = sum(students[2]["Charlie"].values())
print(Charlie_total_marks)
print(students[2])



str1 = 'india'
# write a python program to obtain
# ['India','iNdia','inDia','indIa','indiA']
output=[]

for i in range(len(str1)):
    temp = str1[:i].lower()+str1[i].upper()+str1[i+1:].lower()
    output.append(temp)
print(output)
    

