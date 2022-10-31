
## Todays agenda

```
Review 4a's assignment
Continue 4a assignment, maybe using the hints provided here.
Todays assignment.
Final assignment presentation
```

## Last lectures assignment:

```
You should create a program that takes two arguments.
The first argument is a file with flight information.
The second argument is a file with booking information.
Your program should create files with a ticket for each booking.
```

**Example running the program:**
```
4a-tickets.exe flights.csv booking.csv
```

**Will produce the files:**

```
ticket-1001.txt ticket-1002.txt ticket-1003.txt ticket-1004.txt
ticket-1005.txt ticket-1006.txt ticket-1007.txt ticket-1008.txt
... etc

```

**Information:**

```
Row seating is 2-3-2 the all flights
```

## Flight data-file structure:

### flights.csv:
```
flightnumber,departure,destination,date,time,fseats,bseats,eseats
```

## Booking data-file structure:

### bookings.csv:
```
bookingnumber,date,time,departure,destination,seatclass,firstname,surname
```

## Output:

The tickets should be written to files in the format:
```
ticket-{bookingnumber}.txt
```

Each file should contain the following information in this format: 
```
BOOKING:{bookingnumber} 
FLIGHT:{flight} DEPARTURE:{dep} DESTINATION: {dest} {date} {time}
PASSENGER {firstname} {surname}
CLASS: {seatclass}
ROW {row} SEAT {seatnumber}
```

### Example of ticket filename:

```
ticket-2007.txt
```

### Example of ticket file content:
```
BOOKING:2007
FLIGHT:304 DEPARTURE:GOT DESTINATION:CPH 2022-10-27 06:30
PASSENGER: Kalle Kula
CLASS: first
ROW:4 SEAT:24
```

## Hint's for completing the assignment:

### Hint 1, use pseudo code:
A good tip is to always start writing high level pseudo code of what your program/function should do.
A short description that you can communicate with others without the C syntax knowledge.
Writing pseudo code is also a part of the process creating software, that prepares your brain for the task.
Breaking down the requirements into small defined tasks that your(and other) brains can easily read and understand.
When you get experienced with C, you might find that the C-code itself is so natural to you it is just as natural and easy to understand like the pseudo code itself.

#### Example
```
READ INDATA FLIGHTS FILE AND SAVE IT IN A FLIGHTS LIST.
READ INDATA BOOKING FILE AND SAVE IT IN A BOOKING LIST.
FOR EACH BOOKING IN THE BOOKING LIST
    FIND THE FLIGHT THE BOOKING IS ON
    THEN ALLOCATE A SEAT 
    MARK THE SEAT BOOKED
    THEN PRINT THE TICKET TO A TICKET FILE
```

### Hint 2, create data structures that map the indata
```
typedef struct flight_list_node {
	int flightno;          /* !< The flight number */
	char dep[20];          /* !< Departure airport code */
	char des[20];          /* !< Destination airport code */
	char datestr[20];      /* !< Date departure*/
	char timestr[20];      /* !< Time departure */
	int nfs;               /* !< Number of First class rows */
	int *fs;	       /* !< An array with flags of if a seat is taken or not. 1==occupied, 0==not occupied */
	int nbs;               /* !< Number of Business class rows */
	int *bs;               /* !< An array with flags of if a seat is taken or not. 1==occupied, 0==not occupied */
	int nes;               /* !< Number of Economy class rows */
	int *es;               /* !< An array with flags of if a seat is taken or not. 1==occupied, 0==not occupied */
	struct flight_list_node *next; /* !< A pointer to the next flight information */
} FlightListNode;

typedef struct booking_list_node {
	int booking;          /* !< The booking number */
	char datestr[15];     /* !< The departure date */
	char timestr[15];     /* !< The departure time */
	char dep[10];         /* !< The departure airport */
	char des[10];         /* !< The destination airport */
	char class[20];       /* !< The seat class */
	char fname[25];       /* !< Firstname */
	char lname[25];       /* !< Lastname */
	struct booking_list_node *next; /* !< a pointer to the next booking */
} BookingListNode;

```

### Hint 3, example of realizing READ INDATA FLIGHTS FILE AND SAVE IT IN A FLIGHTS LIST
```
FlightListNode *read_flights(const char *filename)
{
	FlightListNode fln,*head=NULL;
	FILE *fp = fopen(filename,"r");
	while( fscanf(fp,"%d,%[^,],%[^,],%[^,],%[^,],%d,%d,%d\n",&fln.flightno,fln.dep,fln.des,fln.datestr,fln.timestr,&fln.nfs,&fln.nbs,&fln.nes) == 8 ) {
		FlightListNode *nn = malloc(sizeof(FlightListNode));
		memcpy(nn,&fln,sizeof(FlightListNode));
		nn->fs = malloc(fln.nfs*sizeof(int)*7); /** 7 is the number of seats in a row */
		memset(nn->fs,0,fln.nfs);
		nn->bs = malloc(fln.nbs*sizeof(int)*7); /** 7 is the number of seats in a row */
		memset(nn->bs,0,fln.nbs);
		nn->es = malloc(fln.nes*sizeof(int)*7); /** 7 is the number of seats in a row */
		memset(nn->es,0,fln.nes);
		nn->next =  head;
		head = nn;
	}
	return(head);
}
```

### Hint 4, example of realizing READ INDATA BOOKING FILE AND SAVE IT IN A BOOKING LIST.
```
BookingListNode *read_bookings(const char *filename) 
{
	/* code the same way as when reading the flights and create a list */
{
```

### Hint 5, example of realizing FOR EACH BOOKING IN THE BOOKING LIST
```
int create_tickets(BookingListNode *bookings, FlightListNode *flights)
{
	int num_tickets = 0;
	fprintf(stdout,"Writing tickets: ");
	for(BookingListNode *blnp = bookings; blnp != NULL; blnp=blnp->next) {
            /* find the flight for this booking */
	}
	fprintf(stdout,"Created  %d tickets\n\n",num_tickets);
	return(num_tickets);
}
```

### Hint 6, example of realizing FIND THE FLIGHT THE BOOKING IS ON
```
		for(FlightListNode *flnp = flights; flnp != NULL; flnp=flnp->next) {
			if( !strcmp(blnp->dep,flnp->dep) &&  !strcmp(blnp->des,flnp->des) &&  !strcmp(blnp->datestr,flnp->datestr) &&  !strcmp(blnp->timestr,flnp->timestr) )
			{
                             /* allocate seat and mark it booked */
			}
		}
```


### Hint 7, example of realizing THEN ALLOCATE A SEAT AND PRINT THE TICKET
```
				int row=0, seat=0;
				if(allocate_seat(flnp,blnp,&row,&seat)){
					fprintf(stdout,"[ticket-%d.txt]",blnp->booking);
					print_ticket(blnp,flnp,seat,row);
					num_tickets++;
				}
```


### Hint 8, example of realizing THEN ALLOCATE A SEAT && MARK THE SEAT BOOKED
```
int allocate_seat(FlightListNode *flight,BookingListNode *booking,int *row, int *seat)
{
	int sn = 0;
	int rn = 0;
	if( strcmp("first",booking->class) == 0 ) {
		for(int p=0;p<flight->nfs*7;p++) {
			if( flight->fs[p] == 0 ) {
				flight->fs[p] = 1;
				sn = p+1;
				rn = (int)p/7+1;
				break;
			}
		}
	}
	if( strcmp("business",booking->class) == 0 ) {
		for(int p=0;p<flight->nbs*7;p++) {
			if( flight->bs[p] == 0 ) {
				flight->bs[p] = 1;
				sn = p+flight->nfs*7+1;  /* humans usually start counting at 1 */
				rn = flight->nfs+(int)p/7+1;
				break;
			}
		}
	}
	if( strcmp("economy",booking->class) == 0 ) {
		for(int p=0;p<flight->nes*7;p++) {
			if( flight->es[p] == 0 ) {
				flight->es[p] = 1;
				sn = p+flight->nfs*7+flight->nbs*7+1;  /* humans usually start counting at 1 */
				rn = flight->nfs+flight->nbs+(int)p/7+1;
				break;
			}
		}
	}
	if( rn ==0 || sn == 0 ) {
		fprintf(stdout,"did not find class \"%s\" for booking on this plane\n",booking->class);
		return(0);
	}
	*row = rn;
	*seat = sn;
	return(1);
}
```

### Hint 9, example of realizing PRINT THE TICKET TO A TICKET FILE
```
void print_ticket(BookingListNode *blnp, FlightListNode *flnp,int seat,int row)
{
	char filename[255];
	sprintf(filename,"ticket-%d.txt",blnp->booking);
	FILE *fp = fopen(filename,"w");
	if( fp ) {
		fprintf(fp,"BOOKING:%d\n",blnp->booking);
		fprintf(fp,"FLIGHT:%d DEPARTURE:%s DESTINATION: %s %s %s\n",flnp->flightno,flnp->dep,flnp->des,flnp->datestr,flnp->timestr);
		fprintf(fp,"PASSENGER %s %s\n",blnp->fname,blnp->lname);
		fprintf(fp,"CLASS: %s\n",blnp->class);
		fprintf(fp,"ROW %d SEAT %d\n\n",row,seat);
		fclose(fp);
	}
}
```

## Assignments for today (after finished with the above assigment).
Change your code by adding this functionality.

### 1. Cancel all flights that does not have any passengers.
It is not good for the business to run planes without passengers. These flights must be cancelled.
Create a function that checks all flights seatings. If there is no occupied seats on a flight it should
be cancelled.

### 2. Create a report-file of cancelled flights named cancelled-flights.txt .
The management want's to see a report of how many and which flights are cancelled.
(Hmm, maybe create this report when cancelling the flight?)

### 3. Create a seating map for each flight and save these into a seating-report.txt file.
Booking staff want's to know where there are free seatings on the flights.
The seating map should show each section and rows in the section.
A '1' will indicate a seat is taken. '0' indicates it is free.

```
Example:

Flight 234, Departure ARN, Destination GOT, Date 2022-02-03, Time 07:20
first class
[1][1] [1][1][1] [1][1]
[1][1] [1][1][1] [1][1]
[1][1] [1][0][0] [0][0]
[0][0] [0][0][0] [0][0]
[0][0] [0][0][0] [0][0]
business class
[1][1] [1][1][1] [1][1]
[1][1] [1][1][1] [1][1]
[1][1] [1][1][1] [1][1]
[0][0] [0][0][0] [0][0]
[0][0] [0][0][0] [0][0]
economy class
[1][1] [1][1][1] [1][1]
[1][1] [1][1][1] [1][1]
[1][1] [1][1][1] [1][1]
[1][1] [1][1][1] [1][1]
[1][1] [1][1][1] [1][1]
[1][1] [1][1][1] [1][1]
[1][1] [1][0][0] [0][0]
[0][0] [0][0][0] [0][0]
```

### 4. Read about the getopt() function and make a small example of using it.
Getting directions input to a program is a standard way of telling the program how to operate.

### 5. Create a program that creates "fake flights" into a .csv file
Create some options for the fake flight program to use when creating fake flights.
Example is to set a range of dates, minimum, maximum seats for each class .... etc

Useful standard functions to use could be strtok, and the functions that manipulates time in C.
```
fake-flights.exe -?
 -s {start date}
 -e {end date}
 -n {number of flights per day}
 -t {list of departure times that can be used}
 -f {list of possible number of rows in first class}
 -b {list of possible number of rows in business class}
 -e {list of possible number of rows in economy class}
 -c {list seating configurations}
 -o {output filename}
```

### 6. Create a program that creates "fake bookings" to the flights in flights.csv file
Let the fake bookings generator know which flights file to read and create bookings for by specifying a filename argument option.
Create arguments that tells the fake bookings generator how many fake bookings it should create.
Be creative and add other options to the program.
Also be creative and make random names!

### 7. Use the fake-flights-generator to create a new flights.csv file
```
fake-flights.exe -s "2022-01-01" -e "2022-01-31" -n 5 -t "06:00,07:00,10:00,13:00,18:00" -f "5,10,15" -b "10,15,20" -e "20,30,40" -c "2-3-2,2-2-2,3-3-3" -o fake-flights.csv
```

### 8. Use the fake-booking-generator to create a new bookings.csv file from the new flights.csv
```
fake-bookings.exe -f fake-flights.csv -o fake-bookings.csv
```

### 9. Run your create tickets program with the new flights.csv and bookings.csv and verify the output.
```
tickets -f flights.csv -b bookings.csv
```


# Final assignment for this class

## The ticket generator you made in C should be turned into a C++ version.
We will start introducing C++ on Wednesday.
You should create a repository named ticket-system.
The code should be commented in a way that doxygen could produce a nice output for your software (html, pdf is a bonus).
(more requirements will be announced next week).



