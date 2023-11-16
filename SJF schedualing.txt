// C++ program to implement Shortest Remaining Time First
// Shortest Remaining Time First (SRTF)

#include <iostream>
#include<limits.h>
#include<vector>
using namespace std;

struct Process {
	int pid; // Process ID
	int bt; // Burst Time
	int art; // Arrival Time
};



vector<int> Pid;
vector<int> T;
// Function to find the waiting time for all
// processes
void findWaitingTime(Process proc[], int n, int wt[])
{
	int rt[n];

	// Copy the burst time into rt[]
	for (int i = 0; i < n; i++)
		rt[i] = proc[i].bt;

	int complete = 0, t = 0, minm = INT_MAX;
	int shortest = 0, finish_time;
	bool check = false;

	// Process until all processes gets
	// completed

	while (complete != n) {

		// Find process with minimum
		// remaining time among the
		// processes that arrives till the
		// current time`

		for (int j = 0; j < n; j++) {
			if ((proc[j].art <= t) && (rt[j] < minm) && rt[j] > 0) {
				minm = rt[j];
				shortest = j;
				check = true;
			}
		}

		if (check == false) {
			t++;
			continue;
		}

		// Reduce remaining time by one
		rt[shortest]--;

		// Update minimum
		minm = rt[shortest];
		if (minm == 0)
			minm = INT_MAX;

		// If a process gets completely
		// executed
		if (rt[shortest] == 0) {

			// Increment complete
			complete++;
			check = false;

			// Find finish time of current
			// process
			finish_time = t + 1;

			// Calculate waiting time
			wt[shortest] = finish_time - proc[shortest].bt - proc[shortest].art;

			if (wt[shortest] < 0)
			    wt[shortest] = 0;
		}
		// Increment time
		t++;
		//Printing Gantt Chart
	    	Pid.push_back(shortest);
		T.push_back(t);
	}
}

//Function to print Gantt Chart
void printGanttChart(Process proc[]){
	int min=INT_MAX;
    for(int a=0;a<Pid.size();a++){
        if(min>proc[a].art){
            min=proc[a].art;
        }
    }
    cout<<"\nGantt Chart:\n|";
    for(int i=0;i<Pid.size();i++){
        if(Pid[i]!=Pid[i+1]){
            cout<<"    P"<<Pid[i]+1<<"    |";
        }
    }
    cout<<"\n"<<min;
    for(int i=0;i<Pid.size();i++){
        if(Pid[i]!=Pid[i+1]){
            if(T[i]>=10){
                cout<<"         "<<T[i];
            }
            else{
                cout<<"         "<<"0"<<T[i];
            }
        }
    }
    cout<<"\n\n";
}
// Function to calculate turn around time
void findTurnAroundTime(Process proc[], int n, int wt[], int tat[])
{
	// calculating turnaround time by adding
	// bt[i] + wt[i]
	for (int i = 0; i < n; i++)
		tat[i] = proc[i].bt + wt[i];
}

// Function to calculate average time
void findavgTime(Process proc[], int n)
{
	int wt[n], tat[n], total_wt = 0,
					total_tat = 0;

	// Function to find waiting time of all
	// processes
	findWaitingTime(proc, n, wt);

	// Function to find turn around time for
	// all processes
	findTurnAroundTime(proc, n, wt, tat);

	// Display Gantt Chart and processes along with all details

	printGanttChart(proc);

	for (int i = 0; i < n; i++) {
		total_wt = total_wt + wt[i];
		total_tat = total_tat + tat[i];
		cout << "Process "<< proc[i].pid << ":\n" << "\tWaiting Time:" << wt[i] << "\n\tTurn Around Time:" << tat[i] << "\n\n";
	}

	// Calculate total waiting time and
	// total turnaround time

	cout << "\nAverage waiting time = " << (float)total_wt / (float)n;
	cout << "\nAverage turn around time = " << (float)total_tat / (float)n;
}

// Driver code
int main()
{
	int n;
    cout << "Enter the number of processes: ";
    cin >> n;

    Process proc[n];

    for (int i = 0; i < n; ++i) {
        cout << "Enter arrival time for Process " << i + 1 << ": ";
        cin >> proc[i].art;
        cout << "Enter burst time for Process " << i + 1 << ": ";
        cin >> proc[i].bt;
        proc[i].pid = i + 1;
    }

	findavgTime(proc, n);
	return 0;
}
