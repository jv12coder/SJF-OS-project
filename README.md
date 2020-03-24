//# SJF-OS-project


#include<stdio.h>

#include<stdlib.h>

#include<unistd.h>



#define FILE_NAME "CPU_BURST.txt"



struct Process{

int ArrivalT,BurstT,WaitT,TurnAroundT;

char name[4];

};



struct Process initialize(int ArrivalT,int BurstT,int name){

struct Process X;

X.BurstT = BurstT;

X.ArrivalT = ArrivalT;

sprintf(X.name,"P%d",name+1);



return X;

}



int main(){



FILE *fp = fopen(FILE_NAME,"r");



if(!fp)

return -1*printf("FILE OPEN ERROR!\n");



int d,i,j,count=0;



int *queue = (int*)malloc(sizeof(int));





while(EOF != fscanf(fp,"%d ",&d )){

printf("%d ",d);

queue = (int*)realloc(queue,(count+1)*sizeof(int));

queue[count++] = d;

}

fclose(fp);



//int queue[] = {3,1,3,2,4};



struct Process P[count];



for(i=0; i<count; i++)

P[i] = initialize(0,queue[i],i);



//sort

for(i=1; i<count; i++){

for(j=0; j<count-i; j++){

if(P[j].BurstT>P[j+1].BurstT){

struct Process temp = P[j];

P[j] = P[j+1];

P[j+1] = temp;

}

}

}







printf("\nOrder : ");



int elapsed_time=0;

for(i=0; i<count; i++){

P[i].WaitT= elapsed_time;

P[i].TurnAroundT= P[i].WaitT+P[i].BurstT;

elapsed_time += P[i].BurstT;



printf("%s ",P[i].name);

}

//sort again

for(i=1; i<count; i++){

for(j=0; j<count-i; j++){

if(P[j].name[1]>P[j+1].name[1]){

struct Process temp = P[j];

P[j] = P[j+1];

P[j+1] = temp;

}

}

}

printf("\n\n%7s|%8s|%6s|%5s%s\n","PROCESS","ARRIVAL","BURST","WAIT","TURNAROUND");



int total_WaitT=0,total_TurnAroundT=0;



for(i=0; i<count; i++){

total_WaitT+=P[i].WaitT;

total_TurnAroundT+=P[i].TurnAroundT;

printf("%7s|%8d|%6d|%5d|%9d\n",P[i].name,P[i].ArrivalT,P[i].BurstT,P[i].WaitT,P[i].TurnAroundT);

}



printf("\nAverage Waiting Time : %.2f\n",total_WaitT*1.0/count);

printf("\nAverage Turn Around Time : %.2f\n",total_TurnAroundT*1.0/count);



return 0*printf("\nSUCCESSFUL EXIT\n");

}

