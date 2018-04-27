/*Design a scheduler that can schedule the processes arriving system at periodical
intervals. Every process is assigned with a fixed time slice t milliseconds. If it is not able to
complete its execution within the assigned time quantum, then automated timer generates an
interrupt. The scheduler will select the next process in the queue and dispatcher dispatches the
process to processor for execution. Compute the total time for which processes were in the queue
waiting for the processor. Take the input for CPU burst, arrival time and time quantum from the
user.   */
#include<stdio.h>
#include<stdlib.h>
struct process
{
	int id;int pri;int at;
	int bt;int ubt;int st;int wt;int cmp;int tat;
	
};
void increasepri(struct process p[],int n)
{
	int i;
	 for (i=0;i<n;i++)
	{
		if(p[i].wt!=0&&p[i].wt%2==0)
		{
			if(p[i].pri>1)
			p[i].pri-=1;
		}
	}
}
int main()
{
	int t,p,i,j,r,working=-1,current;
	float avgtat=0,avgwt=0;
	printf("Enter number of processes\n");
	scanf("%d",&p);
	struct process pro[p+1];
	printf("For the Processes Enter the Arrival t Burst t Priority\n");
	for(i=0;i<p;i++)
	{
		printf("For process P%d ",i+1);
		pro[i].id=i+1;
		scanf("%d",&pro[i].at);
		scanf("%d",&pro[i].bt);
		scanf("%d",&pro[i].pri);
		pro[i].wt=0;
		pro[i].st=-1;
		pro[i].ubt=pro[i].bt;
	}
	pro[p].pri=INT_MAX;
	printf("\nP AT BT Priority\n");
	for(i=0;i<p;i++)
	{
		printf("%d  %d  %d  %d\n",pro[i].id,pro[i].at,pro[i].bt,pro[i].pri);
	}
	t=0;
	working=p;
	r=p;
	while(r!=0)
	{
		for(i=0;i<p;i++)
		{	
			if(pro[i].at<=t && pro[i].ubt!=0 && pro[i].pri<pro[working].pri)
			{
				
				working=i;
				if(i!=working)
				pro[i].wt+=1;
			}
		}
		if(pro[working].st==-1)
		{
			if(r!=p)
			{
			pro[working].st=t+2;
			}
			else
			pro[working].st=t;
		}
	
		pro[working].ubt-=1;
		current=working;
		t=t+1;
		increasepri(pro,p);
		 if(pro[working].ubt==0)
		{
			pro[working].cmp=t;
			t+=2;
			current=working;
			increasepri(pro,p);
			working=p;
			r--;
		}
		if(r!=10&&current!=working)
		{
			t+=2;
			current=working;
		}
	}
	printf("\n\nProcess   AT   BT   CT   TAT   WT\n");
	for(i=0;i<p;i++)
	{
		pro[i].tat=pro[i].cmp-pro[i].at;
		avgtat=avgtat+pro[i].tat;
		pro[i].wt=pro[i].tat-pro[i].bt;
		avgwt=avgwt+pro[i].wt;
		printf("%5d   %5d   %4d   %4d   %4d   %4d\n",pro[i].id,pro[i].at,pro[i].bt,pro[i].cmp,pro[i].tat,pro[i].wt);
	}
	avgwt=(avgwt*1.0)/(p*1.0);
	avgtat=(avgtat*1.0)/(p*1.0);
	printf("Average wting t is %0.2f\nAverage TAT is %0.2f\n",avgwt,avgtat);
}
