#include <algorithm>
#include <fstream>
#include <vector>
#include <queue>
#include <iostream>

using namespace std;

struct Result{
    int finishTime;
    int hairDresserIndex;
    int customerIndex;

    bool operator<(const Result& other) const {  
        return finishTime > other.finishTime; 
    }
};

struct Customer{
    
    int comingTime;
    int index;
    int duration;

    bool operator<(const Customer& other) const {  
        return comingTime > other.comingTime; 
    }
};

struct Hairdresser {
    
    int freeTime;
    int index;
    
    bool operator<(const Hairdresser& other) const {
        if (freeTime != other.freeTime) {
           return freeTime > other.freeTime;
        } else {
           return index > other.index;
        }
    }
};

bool isInTheBreak(int n, int time, int duration){
    return (time/100) % 10 == n || ((time+duration) / 100) % 10 == n;
}


int main(){
    ifstream inputFile("hair.in");
    ofstream outputFile("hair.out");

    int numberOfHairdressers;
    inputFile >> numberOfHairdressers;

    priority_queue<Customer> customers;
    priority_queue<Hairdresser> hairdressers;
    priority_queue<Result> results;

    for(int i=0; i<numberOfHairdressers; i++){
        hairdressers.push({0,i+1});
    }

    //vector<pair<int, pair<int, int>>>finished_customers;
    int timeCustomer, numCustomer, durCustomer;

    while(inputFile >> timeCustomer >> numCustomer >> durCustomer){
        if(timeCustomer == 0){
            break;
        }
        customers.push({timeCustomer,numCustomer,durCustomer});
    }

    long endOfDay = 2000000000;

    while(!customers.empty()){
    
        Customer current_customer = customers.top();
        customers.pop();
        Hairdresser current_hairdresser = hairdressers.top();
        hairdressers.pop();


        bool isInBreak = isInTheBreak(current_hairdresser.index, current_customer.comingTime, current_customer.duration);

        int nextFreeTime = 0;

        if(isInBreak){
            int breakStartTime = ((current_customer.comingTime +current_customer.duration)/100)*100;
            int breakEndTime = breakStartTime+100;
            nextFreeTime = breakEndTime + current_customer.duration;          
        }else{
            nextFreeTime = max(current_hairdresser.freeTime, current_customer.comingTime) + current_customer.duration;
        }
        if(nextFreeTime>endOfDay) break;

        current_hairdresser.freeTime = nextFreeTime;
        results.push({nextFreeTime-1,current_hairdresser.index, current_customer.index});
        hairdressers.push(current_hairdresser);
    }

    while(!results.empty()){
        Result item  = results.top();
        results.pop();    
        outputFile << item.finishTime << " " << item.hairDresserIndex << " " << item.customerIndex << endl;
    }

    inputFile.close();
    outputFile.close();


    return 0;

}





