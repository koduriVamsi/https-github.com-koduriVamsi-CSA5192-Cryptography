#include<stdio.h>
#include<conio.h>
#include<string.h>
int main()
{
    char pt[20]={'\0'},ct[20]={'\0'},key[20]={'\0'},rt[20]={'\0'};
    int i,j;
    int clrscr();
    printf("\n enter the plain text:");
    scanf("%s",pt);
    printf("\n enter the key:");
    scanf("%s",key);
    j=0;
    for(i=strlen(key);i<strlen(pt);i++)
    {
    if(j==strlen(key))
    {
    j=0;
    }
    key[i]=key[j];
    j++;
    }
    printf("\n new key is:%s",key);
    for(i=0;i<strlen(pt);i++)
    {
        ct[i]=(((pt[i]-97)+(key[i]-97))%26)+97;
    }
    printf("\n \n cipher text is:%s",ct);
    for(i=0;i<strlen(ct);i++)
    {
    if(ct[i]<key[i])
    {
        rt[i]=26+((ct[i]-97)-(key[i]-97))+97;
    }
    else
        rt[i]=(((ct[i]-97)-(key[i]-97))%26)+97;
    }
    printf("\n \n plain text is:%s",rt);
    getch();
}
