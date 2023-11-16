#include <iostream>
#include <vector>
#include <limits.h>
using namespace std;

int main()
{
    int i, j, smallest, count = 0, time, n;
    double avg = 0, tt = 0, end;
    vector<int> Pid;
    vector<int> T;
    cout << "\nEnter the number of Processes: ";
    cin >> n;
    int a[n], b[n], x[n];
    int waiting[n], turnaround[n], p[n + 1];
    for (i = 0; i < n; i++)
    {
        cout << "\nProcess " << i + 1 << ":\n";
        cout << "\tEnter arrival time: ";
        cin >> a[i];
        cout << "\tEnter burst time: ";
        cin >> b[i];
        cout << "\tEnter priority: ";
        cin >> p[i];
    }
    for (i = 0; i < n; i++)
    {
        x[i] = b[i];
    }

    p[n] = INT_MIN; // Set a low initial priority
    for (time = 0; count != n;)
    {
        smallest = n;
        for (i = 0; i < n; i++)
        {
            if (a[i] <= time && p[i] > p[smallest] && b[i] > 0)
                smallest = i;
        }
        if (smallest == n)
        {
            time++;
        }
        else
        {
            time += b[smallest];
            end = time;
            waiting[smallest] = end - a[smallest] - x[smallest];
            turnaround[smallest] = end - a[smallest];
            count++;
            Pid.push_back(smallest);
            T.push_back(time);
            b[smallest] = 0; // Process completes
        }
    }

    int min=INT_MAX;
    for(int i=0;i<Pid.size();i++){
        if(min>a[i]){
            min=a[i];
        }
    }
    cout << "\nGantt Chart:\n|";
    for (int i = 0; i < Pid.size(); i++)
    {
        if (Pid[i] != Pid[i + 1])
        {
            cout << "    P" << Pid[i] + 1 << "    |";
        }
    }
    cout << "\n"<<min;
    for (int i = 0; i < Pid.size(); i++)
    {
        if (Pid[i] != Pid[i + 1])
        {
            if (T[i] >= 10)
            {
                cout << "         " << T[i];
            }
            else
            {
                cout << "         " << "0" << T[i];
            }
        }
    }
    cout << "\n\n";

    for (i = 0; i < n; i++)
    {
        cout << "Process " << i + 1 << ":\n";
        cout << "\tWaiting Time:" << waiting[i] << "\n";
        cout << "\tTurn Around Time:" << turnaround[i] << "\n";
        avg = avg + waiting[i];
        tt = tt + turnaround[i];
    }
    cout << "\n\nAverage waiting time =" << avg / n;
    cout << "\nAverage Turnaround time =" << tt / n << endl;
}
