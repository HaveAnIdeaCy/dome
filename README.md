# student MIS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
struct student_date{
    int num;
    char name[20];
    int math;
    int cyuyan;
    struct student_date *next;
};
typedef struct student_date  STU;
STU *head;
void menu();
void load();//
void save();//
void update();//
void browse();//
void addstu();//
void delstu();  //
void add(STU *p);//
void del(STU *p);//
int main(void)
{
    int op;
    head=(STU*)malloc(sizeof(STU));
    head->next=NULL;
    menu();
    scanf("%d",&op);
    while(op!=0) {
        switch (op) {
            case 1:  load();  break;
            case 2:  save();  break;
            case 3:  addstu();  break;
            case 4:  delstu();  break;
            case 5:  update();  break;
            case 6:  browse();  break;
            default:  op=2;  break;

        }
        menu();
        scanf("%d",&op);
    }
}
void menu()
{
printf("Main Menu\n");
printf(" *******************\n");
printf("*1:load    2:saven  *\n");
printf("*3:add     4:del    *\n");
printf("*5:update  6:browse *\n");
printf("*7:default 0:quit   *\n");
printf(" *******************\n");
}
void addstu()
{
    STU *date;
    char num,name;
    int math,cyuyan;
    date=(STU*)malloc(sizeof(STU));
    printf("input num,name,math,cyuyan:\n");
    scanf("%d%s%d%d",&date->num,date->name,&date->math,&date->cyuyan);
    date->next=NULL;
    add(date);
}
void add(STU *p)
{
    STU *q,*q1,*q2;
    q=p;
    q1=head;
    q2=head->next;
    if(q2==NULL)
        head->next=p;
    else{
        while((q->num>q2->num) && (q2->next!=NULL))
        {
           q1=q2;
           q2=q2->next;
        }
        if(q->num<=q2->num)
        {
            q1->next=q;
            q->next=q2;}

        else
            q2->next=q;

    }

}
void delstu()
{
   int num,temp;
   char name[20];
   STU *p;
   p=head->next;
   printf("input 1(num) or 2(name):\n");
   scanf("%d",&temp);
   if(temp==1) {
       printf("input num:\n");
       scanf("%d",&num);
      while(p!=NULL){
          if(p->num==num)  del(p);
          else p=p->next;
      }
      if(p==NULL)  printf("no records\n");
   }
   else if(temp==2){
       printf("input name:\n");
       scanf("%s",name);
       while(p!=NULL){
           if(strcmp(p->name,name))  del(p);
           else p=p->next;
       }
       if(p==NULL)  printf("no records\n");
   }
   else   printf("input 1 or 2\n");

}
void del(STU *p)
{
    STU *q,*q1,*q2;
    q=p;
    q1=head;
    q2=head->next;
    if(q2==NULL)
        printf("it is NULL\n");
    else {
            while (q2 != NULL) {
                if (q == q2) {
                    q1->next=q2->next;
                    free(q2);
                } else {
                    q1 = q2;
                    q2 = q2->next;
                }
            }
        }

}
void load()
{
    FILE *fp;
    fp=fopen("data.txt","r");
    if(fp==NULL){
        printf("打开错误，按任意返回\n");
        getchar();
        return;
    }
    while(!feof(fp))
    {
        STU *node=(STU*)malloc(sizeof(STU));
        node->next=NULL;
        fscanf(fp,"%d%s%d%d\n",&node->num,node->name,&node->math,&node->cyuyan);
        add(node);
    }
    printf("ok\n");
    fclose(fp);
}
void save()
{
    FILE *fp;
    STU *node;
    fp=fopen("data.txt","w");
    node=head->next;
    while(node!=NULL){
        fprintf(fp,"%d %s %d %d  \n",node->num,node->name,node->math,node->cyuyan);
        node=node->next;
    }
    fclose(fp);
}
void update()
{
   char name[20];
   int flag=0;
   STU *p;
   p=head->next;
   printf("要修改人的姓名：\n");
   scanf("%s",name);
   while(p!=NULL) {
       if (strcmp(p->name, name)==0)
       {
         printf("input num,math,cyuyan:\n");
         scanf("%d%d%d",&p->num,&p->math,&p->cyuyan);
         flag=1;
         break;
       }
       p=p->next;
   }
}
void browse()
{
    STU *node;
    node=head->next;
    printf("num\tname\tmath\tcyuyan\n");
    while(node)
    {
        printf("%d\t%s\t%d\t%d\n",node->num,node->name,node->math,node->cyuyan);
        node=node->next;
    }
    printf("\n");
}
