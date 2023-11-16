#include<stdio.h>
#include<conio.h>
int main(){
	int c,p,q,n,n1,i,j,m=5,result=0,d[1000],result2=0,temp;
	printf("Enter a value : ");
	scanf("%d",&p);
	printf("Enter another value : ");
	scanf("%d",&q);
	n=p*q;
	printf("Value of n = %d\n",n);
	n1=(p-1)*(q-1);
	printf("Value of n1 = %d\n",n1);
	int e[10]={3,5,7,11,13,17};
	for(i=0;i<e[i];i++){
		if(n1%n1==0&&n1%e[i]==0){
			result=e[i];
			break;
		}
	}	
	printf("The value of e is %d\n",result);
	for(i=0;i<e[i] && result2!=1;i++){
	for(j=1;j<1000;j++){
		result2=(j*e[i])%n1;
		if(result2==1){
			break;
		}
	}
	printf("The value of d is %d\n",j);
	}
	temp=(m,result);
	c=temp%n;
	printf("Encrypted value : %d",c);
}
