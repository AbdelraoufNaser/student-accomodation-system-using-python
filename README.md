# student-accomodation-system-using-python
def studentRegistration(occupants,A,B,masterRooms,replacedA,replacedB,replacedMaster):
    details = []
    name = input('Enter your name: ')
    TP = input('Enter your TP number: ')
    print("""Apartment types:
1)  Type A:2 single rooms (400 RM)
2)  Type B:2 single rooms (300 RM) - 1 Master bedroom (500 RM)
NOTICEE: All types have kitchen and laundry room  """)
    details.append(name.upper())
    details.append(TP)
    i=1
    while i ==1:
        choice = input('Enter your choice(A or B): ')
        while choice.upper() != 'A' and choice.upper()!='B':
              print('Wrong input')
              choice = input('Enter your choice(A or B): ')
        if choice.upper() == 'B':
            room = input("""Enter room type:
                (A) for a master room
                (b) for a single room
                Your choice: """)
            if room.upper() == 'A':
                rent = 500
                if len(masterRooms) <= 20:
                    roomChoice=roomForMaster(masterRooms,room)
                else:
                    if len(replacedMaster)>0:
                        roomChoice=replacement(replacedMaster,masterRooms)
                    else:
                        print('Sorry, no available rooms in this type. Try a different one')
                        continue
                i = 2
                details.append(str(rent))
                details.append(roomChoice)
                file = masterRooms
                choice = 'master'
                continue
            elif room.upper() == 'B':
                rent = 300
                if len(B) <= 40:
                    roomChoice = roomNumber(B, choice)
                else:
                    if len(replacedB) > 0:
                        roomChoice = replacement(replacedB, B)
                    else:
                        print('Sorry, no available rooms in this type. Try a different one')
                        continue
                i = 2
                details.append(str(rent))
                details.append(roomChoice)
                file = B
                continue
        elif choice.upper() == 'A':
            rent = 400
            if len(A) <= 40:
                roomChoice = roomNumber(A, choice)
            else:
                if len(replacedA) > 0:
                    roomChoice = replacement(replacedA, A)
                else:
                    print('Sorry, no available rooms in this type. Try a different one')
                    continue
            details.append(str(rent))
            details.append(roomChoice)
            file = A
            i = 2
            continue
    details.append(choice.upper())
    internet=internetSubscription(rent)
    payment = paymentCalculation(rent,internet)
    details.append(str(payment))
    occupants.append(details)
    file.append(details)
    return details
def internetSubscription(rent):
    x = 1
    while x == 1:
        if rent == 400:
            internet = input('Do you want internet connection (50 RM) [yes or no]: ')
            if internet.lower() == 'yes':
                net = 50
                x = 2
                continue
            elif internet.lower() == 'no':
                net = 0
                x = 2
                continue
            else:
                print('Invalid input')
                continue
        else:
            internet = input('Do you want internet connection (40 RM) [yes or no]: ')
            if internet.lower() == 'yes':
                net = 40
                x = 2
                continue
            elif internet.lower() == 'no':
                net = 0
                x = 2
                continue
            else:
                print('Invalid input')
                continue
    return net

def roomNumber(list,type):
    apType=type
    calc=[]
    for i in range(1,21):
     calc.append(i)
     calc.append(i)
    numb=int(calc[len(list)])
    if len(list)%2==0:
        roomNum=f'{apType}-{numb}-1'
    else:
        roomNum=f'{apType}-{numb}-2'
    return roomNum
def roomForMaster(list,room):
    num=str(len(list)+1)
    roomNumb=f'master-{num}'
    return roomNumb
def serchByName(occcupants):
    name=input('Enter a name: ')
    for studentFile in occcupants:
        if name.upper() in studentFile :
            writtenDetails=['Name: ','TP Number: ','Rent: ','Room Number: ','apartment type:','Total paid: ']
            i=-1
            for details in studentFile:
                i=i+1
                print(writtenDetails[i],'\t',details)
    return details
def searchByTypeAndRoom(occupants):
    apartment=input('Enter room type[A-B-Master]: ')
    room=input('Enter your room number: ')
    for studentFile in occupants:
        if room in studentFile :
            writtenDetails=['Name: ','TP Number: ','Rent: ','Room Number: ','Apartment type: ','Total paid: ']
            i=-1
            for details in studentFile:
                i=i+1
                print(writtenDetails[i],'\t',details)
    return details




def paymentCalculation(rent,net):
    totalCharges = ((rent + net) * 5) + 100
    print('Your total is: ', totalCharges, '[Notice: 100 RM deposit included]')
    payment = input('Are you going to pay full or partial? :')
    while payment.lower() != 'partial' and payment.lower() != 'full':
          print("wrong input")
          payment = input('Are you going to pay full or partial? :')
    if payment.lower() == 'partial':
        print('You are required to pay at least 50% of total rent + 100 RM deposit')
        paid = int(input('Enter the amount you are going to pay: '))
        while not paid >= ((rent + net) * 2.5) + 100:
            print('Wrong input')
            paid = int(input('Enter the amount you are going to pay: '))
        totalPaid = paid
    if payment.lower() == 'full':
        totalPaid = totalCharges
    return totalPaid

def saveInformation(occupants):
    fileHandler=open('details.txt','w')
    if len(occupants)==1:
        fileHandler.write('Name          TP Number         Rent        Room Number        apartment type    Total paid')
        fileHandler.write('\n')
    for det in occupants :
        for info in det:
            fileHandler.write(info)
            fileHandler.write('\t\t')
        fileHandler.write('\n')
    fileHandler.close()
    return fileHandler
def totalDepositCollected(occupants):
    deposit=len(occupants)*100
    return print(deposit)
def totalAmount(occupants):
    name = input('Enter a name: ')
    for studentFile in occupants:
        if name.upper() in studentFile:
            total=int(studentFile[5])-100
    return print(total)
def totalFromAll(occupants):
    totalAmount=0
    for students in occupants:
        totalAmount+=int(students[5])
    print(totalAmount)
    return totalAmount
def checkingOut(occupants,A,B,master,name):
    for studentFile in occupants:
        if name.upper() in studentFile:
            if studentFile[4]=='A':
                file=A
            elif studentFile[4]=='B':
                file=B
            else:
                file=master
            file.append(studentFile)
            occupants.remove(studentFile)
            total=int(studentFile[5])-100
            rent=int(studentFile[2])
            months=int(input('How many months have you stayed?: '))
            calc=months*rent
            if total>calc:
                extra=total-calc
                print(extra,'RM will be paid back to you[+100Rm deposit]')
            else:
                extra=calc-total
                print('You are required to pay',extra,'RM [You will get the deposit when you pay the required amount]')
    f = open('details.txt')
    new_list = []
    for line in f:
        line.rstrip()
        new_list.append(line)
        if line.startswith(name):
            new_list.remove(line)
    f.close()
    return new_list
def replacement(list,choice):
    for dets in choice :
        if dets==list[0]:
            room=dets[3]
            choice.remove(dets)
    return room
def menu():
    occupants = []
    typeA = []
    typeB = []
    masterRooms = []
    replacedA=[]
    replacedB=[]
    replacedMaster=[]
    i=1
    while i==1:
        print('Select the operation you want to perform: ')
        print('1.For housing registration ')
        print('2.For checking out')
        print('3.For printing total deposit')
        print('4.For printing total amount excluding the deposit')
        print('5.For printing Total amount received from students')
        print('6.For searching by name')
        print('7.For searching by room type and room number ')
        print('8.To exit')
        choice = input('Enter your choice: ')
        if choice=='1':
            studentRegistration(occupants,typeA,typeB,masterRooms,replacedA,replacedB,replacedMaster)
            saveInformation(occupants)
        elif choice=='2':
            name=input('Enter a name: ')
            newList=checkingOut(occupants,replacedA,replacedB,replacedMaster,name.upper())
            file = open('details.txt', 'w')
            for dets in newList:
                file.write(dets)
                file.write('\n')
            file.close()
        elif choice=='3':
            totalDepositCollected(occupants)
        elif choice=='4':
            totalAmount(occupants)
        elif choice=='5':
            totalFromAll(occupants)
        elif choice=='6':
            serchByName(occupants)
        elif choice=='7':
            searchByTypeAndRoom(occupants)
        elif choice=='8':
            i=2
            continue
        else:
            print('Wrong choice')
            continue


menu()
