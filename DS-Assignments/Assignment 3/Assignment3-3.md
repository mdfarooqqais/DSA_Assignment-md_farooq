Assignment No 3.3
# Title : Develop a C++ program to store and manage an appointment schedule for a single day. Appointments should be scheduled randomly using a linked list. The system must define the start time, end time, and specify the minimum and maximum duration allowed for each appointment slot. The program should include the following operations: a) Display the list of currently available time slots b) Book a new appointment within the defined time limits. c) Cancel an existing appointment after validating its time, availability, and correctness. d) Sort the appointment list in order of appointment times. e) Sort the list based on appointment times using pointer manipulation (without swapping data values).

# Theory 
An appointment scheduling system manages time slots for a single day by organizing appointments dynamically. A linked list is an appropriate data structure for this system because appointments can be inserted, deleted, or rearranged easily without shifting elements, unlike arrays.

Each node in the linked list represents an appointment slot containing information such as start time, end time, and duration. The system enforces minimum and maximum duration limits to ensure that appointments are valid and do not overlap incorrectly. Initially, appointments may be scheduled randomly, and the linked list grows or shrinks as bookings and cancellations occur.

The system provides operations to display available time slots, book new appointments, and cancel existing ones after proper validation of tim
e constraints and availability. To maintain order and improve readability, appointments can be sorted based on time. Sorting is implemented in two ways: one using logical rearrangement and another using pointer manipulation, where node links are changed instead of swapping data.

Overall, using a linked list makes the appointment scheduler flexible, efficient, and suitable for real-time scheduling applications such as clinics, offices, or service centers.

# CODE
```cpp
#include <iostream>
#include <string>
#include <cstdlib>
#include <ctime>
#include <iomanip>
using namespace std;

struct Node {
    int start;      
    int end;        
    string desc;
    Node* next;
    Node(int s=0,int e=0,const string &d="") : start(s), end(e), desc(d), next(NULL) {}
};

int timeStrToMins(const string &t) {
    int hh=0, mm=0;
    sscanf(t.c_str(), "%d:%d", &hh, &mm);
    return hh*60 + mm;
}

string minsToTimeStr(int m) {
    int hh = m / 60;
    int mm = m % 60;
    char buf[6];
    sprintf(buf, "%02d:%02d", hh, mm);
    return string(buf);
}

bool overlap(int s1,int e1,int s2,int e2) {
    return !(e1 <= s2 || s1 >= e2);
}

void appendNode(Node*& head, Node* node) {
    if(!head) { head = node; node->next = NULL; return; }
    Node* p = head;
    while(p->next) p = p->next;
    p->next = node;
    node->next = NULL;
}

void freeList(Node*& head) {
    Node* p = head;
    while(p) {
        Node* q = p->next;
        delete p;
        p = q;
    }
    head = NULL;
}

int countNodes(Node* head) {
    int c=0; for(Node* p=head; p; p=p->next) c++; return c;
}

void displayAppointments(Node* head) {
    cout << "\nScheduled Appointments:\n";
    if(!head) { cout << "  [No appointments scheduled]\n"; return; }
    for(Node* p=head; p; p=p->next) {
        cout << "  " << minsToTimeStr(p->start) << " - " << minsToTimeStr(p->end)
             << "  (" << (p->end - p->start) << " mins)" 
             << "  : " << p->desc << "\n";
    }
}

void displayAvailableSlots(Node* head, int dayStart, int dayEnd) {
    cout << "\nAvailable Time Slots:\n";
    
    int prevEnd = dayStart;
    Node* p = head;
    if(!p) {
        cout << "  " << minsToTimeStr(dayStart) << " - " << minsToTimeStr(dayEnd)
             << "  (" << (dayEnd - dayStart) << " mins) [entire day free]\n";
        return;
    }
    while(p) {
        if(p->start > prevEnd) {
            cout << "  " << minsToTimeStr(prevEnd) << " - " << minsToTimeStr(p->start)
                 << "  (" << (p->start - prevEnd) << " mins)\n";
        }
        if(p->end > prevEnd) prevEnd = p->end;
        p = p->next;
    }
    if(prevEnd < dayEnd) {
        cout << "  " << minsToTimeStr(prevEnd) << " - " << minsToTimeStr(dayEnd)
             << "  (" << (dayEnd - prevEnd) << " mins)\n";
    }
}


bool hasOverlap(Node* head, int start, int end) {
    for(Node* p = head; p; p = p->next)
        if(overlap(start,end,p->start,p->end)) return true;
    return false;
}


void addAppointment(Node*& head, int start, int end, const string &desc) {
    Node* node = new Node(start,end,desc);
    appendNode(head, node);
}


bool bookAppointment(Node*& head, int dayStart, int dayEnd, int minDur, int maxDur, int start, int duration, const string &desc) {
    int end = start + duration;
    if(start < dayStart || end > dayEnd) {
        cout << "Error: appointment outside working hours.\n";
        return false;
    }
    if(duration < minDur || duration > maxDur) {
        cout << "Error: duration must be between " << minDur << " and " << maxDur << " minutes.\n";
        return false;
    }
    if(hasOverlap(head,start,end)) {
        cout << "Error: requested slot overlaps existing appointment.\n";
        return false;
    }
    addAppointment(head,start,end,desc);
    cout << "Booked: " << minsToTimeStr(start) << "-" << minsToTimeStr(end) << " : " << desc << "\n";
    return true;
}


bool cancelAppointment(Node*& head, int start, const string &optDesc="") {
    if(!head) return false;
    Node* cur = head;
    Node* prev = NULL;
    while(cur) {
        if(cur->start == start && (optDesc.empty() || cur->desc == optDesc)) {
            if(prev==NULL) { 
                head = cur->next;
            } else {
                prev->next = cur->next;
            }
            cout << "Cancelled: " << minsToTimeStr(cur->start) << "-" << minsToTimeStr(cur->end) << " : " << cur->desc << "\n";
            delete cur;
            return true;
        }
        prev = cur;
        cur = cur->next;
    }
    return false;
}

void sortByData(Node*& head) {
    for(Node* i=head; i; i=i->next) {
        Node* minNode = i;
        for(Node* j=i->next; j; j=j->next) {
            if(j->start < minNode->start) minNode = j;
        }
        if(minNode != i) {
            
            swap(i->start, minNode->start);
            swap(i->end, minNode->end);
            swap(i->desc, minNode->desc);
        }
    }
}

void sortByPointer(Node*& head) {
    Node* sorted = NULL;
    Node* tail = NULL;
    while(head) {
        
        Node* minN = head; Node* minPrev = NULL;
        Node* cur = head; Node* prev = NULL;
        while(cur) {
            if(cur->start < minN->start) { minN = cur; minPrev = prev; }
            prev = cur;
            cur = cur->next;
        }
        
        if(minPrev == NULL) {
            head = minN->next;
        } else {
            minPrev->next = minN->next;
        }
        
        minN->next = NULL;
        if(sorted == NULL) {
            sorted = tail = minN;
        } else {
            tail->next = minN;
            tail = minN;
        }
    }
    head = sorted;
}


void createRandomSchedule(Node*& head, int dayStart, int dayEnd, int minDur, int maxDur, int count) {
    srand((unsigned)time(NULL));
    int attempts = 0;
    int inserted = 0;
    int maxAttempts = count * 50 + 200; 
    while(inserted < count && attempts < maxAttempts) {
        attempts++;
        int dur = minDur + rand() % (maxDur - minDur + 1);
        int window = dayEnd - dayStart - dur;
        if(window < 0) break; 
        int startOffset = rand() % (window + 1);
        int start = dayStart + startOffset;
        int end = start + dur;
        if(!hasOverlap(head,start,end)) {
            addAppointment(head,start,end,string("Rnd") + to_string(inserted+1));
            inserted++;
        }
    }
    sortByData(head);
}

int promptTimeAsMinutes(const string &prompt) {
    string s;
    cout << prompt;
    cin >> s;
    return timeStrToMins(s);
}

void printMenu() {
    cout << "\n--- Appointment Scheduler Menu ---\n";
    cout << "1. Display scheduled appointments\n";
    cout << "2. Display available time slots\n";
    cout << "3. Book a new appointment\n";
    cout << "4. Cancel an appointment (by start time)\n";
    cout << "5. Sort by swapping node data\n";
    cout << "6. Sort by pointer manipulation (no data swap)\n";
    cout << "7. Exit\n";
    cout << "Enter choice: ";
}

int main() {
    Node* head = NULL;

    cout << "Define working day (24-hour format HH:MM)\n";
    string sStart, sEnd;
    cout << "Day start (e.g. 09:00): "; cin >> sStart;
    cout << "Day end   (e.g. 17:00): "; cin >> sEnd;
    int dayStart = timeStrToMins(sStart);
    int dayEnd   = timeStrToMins(sEnd);
    if(dayEnd <= dayStart) {
        cout << "Invalid day time range.\n";
        return 0;
    }

    int minDur, maxDur;
    cout << "Minimum appointment duration (minutes): "; cin >> minDur;
    cout << "Maximum appointment duration (minutes): "; cin >> maxDur;
    if(minDur > maxDur || minDur <=0) {
        cout << "Invalid durations.\n";
        return 0;
    }

    int initialCount;
    cout << "Number of random initial appointments to generate: "; cin >> initialCount;
    createRandomSchedule(head, dayStart, dayEnd, minDur, maxDur, initialCount);
    cout << "\nInitial schedule created (" << countNodes(head) << " appointments).\n";

    int choice;
    while(true) {
        printMenu();
        cin >> choice;
        if(choice == 1) {
            sortByData(head); // ensure sorted for display
            displayAppointments(head);
        } else if(choice == 2) {
            sortByData(head);
            displayAvailableSlots(head, dayStart, dayEnd);
        } else if(choice == 3) {
            string tstr;
            cout << "Enter desired start time (HH:MM): "; cin >> tstr;
            int start = timeStrToMins(tstr);
            int duration; cout << "Enter duration (minutes): "; cin >> duration;
            string desc; cout << "Enter description/name: "; 
            cin.ignore(); getline(cin, desc);
            if(bookAppointment(head, dayStart, dayEnd, minDur, maxDur, start, duration, desc))
                sortByData(head);
        } else if(choice == 4) {
            string tstr;
            cout << "Enter start time of appointment to cancel (HH:MM): "; cin >> tstr;
            int start = timeStrToMins(tstr);
            cout << "Optionally enter description to match (or press ENTER to skip): ";
            cin.ignore();
            string desc;
            getline(cin, desc);
            bool ok = cancelAppointment(head, start, desc);
            if(!ok) cout << "No matching appointment found.\n";
        } else if(choice == 5) {
            sortByData(head);
            cout << "Sorted by data-swap method.\n";
        } else if(choice == 6) {
            sortByPointer(head);
            cout << "Sorted by pointer-manipulation (no data swap) method.\n";
        } else if(choice == 7) {
            break;
        } else {
            cout << "Invalid choice.\n";
        }
    }

    freeList(head);
    return 0;
}
```
# OUTPUT
Define working day (24-hour format HH:MM)
Day start (e.g. 09:00): 09:00
Day end   (e.g. 17:00): 17:00
Minimum appointment duration (minutes): 30
Maximum appointment duration (minutes): 90
Number of random initial appointments to generate: 3

Initial schedule created (3 appointments).

--- Appointment Scheduler Menu ---
1. Display scheduled appointments
2. Display available time slots
3. Book a new appointment
4. Cancel an appointment (by start time)
5. Sort by swapping node data
6. Sort by pointer manipulation (no data swap)
7. Exit
Enter choice: 1

Scheduled Appointments:
  09:30 - 10:30  (60 mins)  : Rnd1
  11:00 - 11:30  (30 mins)  : Rnd2
  14:00 - 15:00  (60 mins)  : Rnd3


--- Appointment Scheduler Menu ---
Enter choice: 2

Available Time Slots:
  09:00 - 09:30  (30 mins)
  10:30 - 11:00  (30 mins)
  11:30 - 14:00  (150 mins)
  15:00 - 17:00  (120 mins)


--- Appointment Scheduler Menu ---
Enter choice: 3
Enter desired start time (HH:MM): 12:00
Enter duration (minutes): 60
Enter description/name: Doctor Visit
Booked: 12:00-13:00 : Doctor Visit


--- Appointment Scheduler Menu ---
Enter choice: 1

Scheduled Appointments:
  09:30 - 10:30  (60 mins)  : Rnd1
  11:00 - 11:30  (30 mins)  : Rnd2
  12:00 - 13:00  (60 mins)  : Doctor Visit
  14:00 - 15:00  (60 mins)  : Rnd3


--- Appointment Scheduler Menu ---
Enter choice: 4
Enter start time of appointment to cancel (HH:MM): 11:00
Optionally enter description to match (or press ENTER to skip): 
Cancelled: 11:00-11:30 : Rnd2


--- Appointment Scheduler Menu ---
Enter choice: 6
Sorted by pointer-manipulation (no data swap) method.


--- Appointment Scheduler Menu ---
Enter choice: 1

Scheduled Appointments:
  09:30 - 10:30  (60 mins)  : Rnd1
  12:00 - 13:00  (60 mins)  : Doctor Visit
  14:00 - 15:00  (60 mins)  : Rnd3


--- Appointment Scheduler Menu ---
Enter choice: 7
