#include<iostream>
#define max 20
using namespace std;

int main()
{
    struct process
    {
        int id,BT,WT,TAT,AT,CT=0;
    }p[20];
    int n,t,k=0,min_ind,completed=0,exe_ind=0,cur_time=0,exe_ord[max],remain[max],current_time[max];
    float ATA=0,AWT=0;
    cout<<"Enter the number of process: ";
    cin>>n;
    cout<<"Enter the time quantum: ";
    cin>>t;
    for (int i = 0; i<n; i++)
    {
        p[i].id=i;
        cout<<"Enter the Burst time for process "<<i<<" :";
        cin>>p[i].BT;
        remain[i]=p[i].BT;
        cout<<"Enter the Arrival time for process "<<i<<" :";
        cin>>p[i].AT;
    }
    while(completed<n)
    {
        current_time[0]=0;
        for(int i=0;i<n;i++)
        {
            if (remain[i]>0 && p[i].AT<=cur_time)
            {
                int time=min(t,remain[i]);
                remain[i]-=time;
                cur_time+=time;
                exe_ord[exe_ind]=i;
                current_time[exe_ind+1]=cur_time;
                exe_ind++;

                if (remain[i]==0)
                {
                    completed++;
                    p[i].CT=cur_time;
                    p[i].TAT=p[i].CT-p[i].AT;
                    p[i].WT=p[i].TAT-p[i].BT;
                    ATA+=p[i].TAT;
                    AWT+=p[i].WT;
                }
            }
        }
    }
    cout<<"GANTT-CHART"<<endl;
    for (int i = 0; i <exe_ind; i++)
    {
        cout<<"  P"<<p[exe_ord[i]].id<<"\t";
    }
    cout<<endl;
    for (int i = 0; i <= exe_ind; i++)
    {
        cout<<current_time[i]<<"\t";
    }
    cout<<endl;
    cout<<endl<<"\nTURN-AROUND TIME "<<endl;
    for (int i = 0; i<n; i++)
    {
        cout<<"Turn-around time of process "<<i+1<<"= "<<p[i].TAT<<" msec"<<endl;
    }
    ATA=ATA/n;
    cout<<"Average Turn-around time = "<<ATA<<" msec"<<endl;
    cout<<endl;
    cout<<endl<<"WAITING TIME "<<endl;
    for (int i = 0; i<n; i++)
    {
        cout<<"Waiting time of "<<i+1<<"= "<<p[i].WT<<" msec"<<endl;
    }
    AWT=AWT/n;
    cout<<"Average Waiting time = "<<AWT<<" msec"<<endl;
    return 0;
}
