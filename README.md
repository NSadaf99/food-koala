# food-koala
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <conio.h>
#include <ctype.h>

#define UP_ARROW 72
#define DOWN_ARROW 80
#define ENTER 13
struct r
{
    int id;
    char name[80];

    char location[80];

    char food[10][80];
    int price [10];

    struct r* next;

}*head;

struct review
{
    char user_id[80];
    int id;
    int rev;
    struct review* next;

}*rev_head;


struct u
{
    char name[30], pass[30];
};

void welcome_screen()
{
    int n;
    printf("\n\n\n\t\t\t1. LOGIN\t\t2. REGISTER\t\t3.MANAGER ACCOUNT");
    printf("\n\n\n\t\t\t\tENTER YOUR CHOICE: ");
    scanf("%d",&n);
    switch(n)
    {
    case 1:
        system("cls");
        login();
        break;
    case 2:
        system("cls");
        reg();
        break;
    case 3:
        system("cls");
        manager();
        break;
    default:
        printf("\n\n\t\t\t\tNO MATCH FOUND\n");
        printf("\n\n\t\t\tPress Enter to re-Enter the choice\n");
        if(getch()==13)
            system("cls");
        welcome_screen();
    }
}
void manager()
{
    char pass[80];
    char test[80];
    int m;

    strcpy(pass,"123");
    FILE *fp2 = fopen("manager.txt", "ab+");
    while(!feof(fp2))
    {
        fread(pass, sizeof(pass), 1, fp2);
    }

    printf("\n\n\n\t\t\t1. LOGIN\t\t2. CHANGE PASSWORD\n\n\n\t\t\t\tENTER YOUR CHOICE:");
    scanf("%d",&m);
    printf("%d",m);
    char c;
    int z=0;
    switch(m)
    {
        case 1:
            system("cls");
            fflush(stdin);
            printf("\n\n\n\t\t\tENTER MANAGER PASSWORD\n\n\n\t\t\t");
            fflush(stdin);
            z=0;
             while((c=getch())!=13)
            {
            test[z++]=c;
            printf("%c",'*');
            }
            test[z]='\0';
            if(strcmp(test,pass)==0){
                system("cls");
                printf("\n\n\n\t\t\tYOU HAVE LOGGED INTO MANAGER ACCOUNT\nPRESS ANY KEY TO CONTINUE\n");
                getch();
                fflush(stdin);
                system("cls");
                manager_menu();
            }
            else
                printf("\n\n\n\t\t\tWRONG PASSWORD\n");
                return;
            break;
        case 2:
            system("cls");
            printf("\n\n\n\t\t\tENTER MANAGER PASSWORD\n\n\n\t\t\t");
            z=0;
             while((c=getch())!=13)
            {
            test[z++]=c;
            printf("%c",'*');
            }
            test[z]='\0';
            if(strcmp(test,pass)==0){
                    printf("\n\n\n\t\t\tENTER NEW PASSWORD");
                    z=0;
                    while((c=getch())!=13)
                    {
                        pass[z++]=c;
                        printf("%c",'*');
                    }
                    pass[z]='\0';
                    //system("cls");
                    printf("\n\n\n\t\t\tMANAGER PASSWORD HAS BEEN CHANGED\n\n\n\t\t\tPRESS ANY KEY\n");
            }
            fwrite(pass,sizeof(pass),1,fp2);
        fclose(fp2);
        char any_key;
        scanf("%d",&any_key);
        fflush(stdin);
        system("cls");
        welcome_screen();

            break;

        default:
        printf("\n\n\t\t\t\tNO MATCH FOUND\n");
        printf("\n\n\t\t\tPress Enter to re-Enter the choice\n");
        if(getch()==13){
            system("cls");
            fflush(stdin);
        welcome_screen();

        }

        return;
    }
}
void reg()
{
    char checker[30], c;
    int n;
    FILE *fp = fopen("web_reg.txt", "ab+");
    printf("\n\n\t\t\t\tCREATE AN ACCOUNT");
    printf("\n\t\t\t\t^^^^^^^^^^^^^^^^^^^^^^^^");
    printf("\n\n\t\t\t\t  ENTER USERNAME: ");
    scanf("%s",checker);
    int alreadyExists = 0;
    int i=0;
    struct u user;
    while(!feof(fp))
    {
        fread(&user, sizeof(user), 1, fp);
        if(strcmp(checker, user.name)==0)
            alreadyExists = 1;
    }
    if(alreadyExists)
    {
        system("cls");
        reg();
    }
    else
    {
        strcpy(user.name, checker);
        printf("\n\n\t\t\t\t  PASSWORD: ");
        int z = 0;
        while((c=getch())!=13)
        {
            user.pass[z++]=c + 1;
            printf("%c",'*');
        }
        fwrite(&user,sizeof(user),1,fp);
        fclose(fp);
        printf("\n\n\tPress enter to confirm Username and Password\n");
        if((c=getch())==13)
        {
            system("cls");
            printf("\n\n\t\tYou have successfully registered\n");
            printf("\n\n\t\tDo you want to log in to your account?\n\n\t\t  ");
            printf("> PRESS 1 FOR YES\n\n\t\t  > PRESS 2 FOR NO\n\n\t\t\t\t");
            scanf("%d",&n);
            switch(n)
            {
            case 1:
                system("cls");
                login();
                break;
            case 2:
                system("cls");
                printf("\n\n\n\t\t\t\t\tTHANK YOU FOR REGISTERING\n");
                welcome_screen();
                break;
            }
        }
        getch();
        fflush(stdin);
    }
}

void login()
{
    FILE *fp = fopen("web_reg.txt", "rb");
    char name[30], pass[30], c;
    int checku, checkp;
    int z = 0;
    printf("\n\n\t\t\t\tLOG IN TO YOUR ACCOUNT");
    printf("\n\t\t\t\t^^^^^^^^^^^^^^^^^^^^^^");
    printf("\n\n\t\t\t\t  ENTER USERNAME: ");
    scanf("%s",name);
    printf("\n\n\t\t\t\t  ENTER PASSWORD: ");
    while((c=getch())!=13)
    {
        pass[z++]=c+1;
        printf("%c",'*');
    }
    pass[z] = 0;
    struct u user;
    int loginSuccessful = 0;
    while(!feof(fp))
    {
        fread(&user, sizeof(user), 1, fp);
        checku = strcmp(name, user.name);
        checkp = strcmp(pass, user.pass);
        if(checku==0&&checkp==0)
        {
            loginSuccessful = 1;
            break;
        }
    }
    if(loginSuccessful)
    {
        system("cls");
        printf("\n\n\n\t\t\tYOU HAVE LOGGED IN SUCCESSFULLY!\n PRESS ANY KEY TO CONTINUE\n");
        getch();
        fflush(stdin);
        system("cls");
        menu();

    }
    else
    {
        printf("\n\n\n\t\t\tWRONG USERNAME or PASSWORD\n");
        printf("\n\n\t\t\t\t  (Press 'Y' to log in again, Press 'N' to register)");
        char d = getch();
        fflush(stdin);
        if(d=='y'||d=='Y')
            login();
        else if(d=='n'||d=='N')
            reg();
    }
}

void search()
{
    char menu_option_lower[3][100];
    char menu_option_upper[3][100];
    strcpy(menu_option_lower[0],"Search by name of restaurant");
    strcpy(menu_option_lower[1],"Search by location");
    strcpy(menu_option_lower[2],"Back");

    strcpy(menu_option_upper[0],"SEARCH BY NAME OF RESTAURANT");
    strcpy(menu_option_upper[1],"SEARCH BY LOCATION");
    strcpy(menu_option_upper[2],"BACK");

    char key;
    char arrow[] = "==>>";

    printf("%s\t%s\n",arrow,menu_option_upper[0]);
    printf("\t%s\n",menu_option_lower[1]);
    printf("\t%s\n",menu_option_lower[2]);
    printf("Press UP and DOWN key to navigate the menu");
    int i = 0;
    int j;
    while(1)
    {
        key = getche();
        if(key == DOWN_ARROW)
        {
            if(i == 2)
                i = 0;
            else
                i++;
        }
        else if(key == UP_ARROW)
        {
            if(i == 0)
                i = 2;
            else
                i--;
        }
        if(key == ENTER)
        {
            if(i == 0)
            {
                system("cls");
                search_by_name();
            }
            else if(i == 1)
            {
                system("cls");
                search_by_location();
            }
            else if(i==2)
                return;
        }
        system("cls");
        for(j = 0; j < 3; j++)
        {
            if(j == i)
                printf("%s\t%s\n",arrow,menu_option_upper[j]);
            else
                printf("\t%s\n",menu_option_lower[j]);
        }
        printf("Press UP and DOWN key to navigate the menu");
    }

}
void search_by_name()
{
    int flag=0;
    printf("Enter name of restaurant: ");
    char str[100];

    gets(str);
    struct r *current;
    current = head;
    int i;
    char action_key;
    if(head == NULL)
    {
        printf("There are no restaurants. Press any key to continue or 'e' to exit");
        action_key = getche();
        if(action_key == 'e')
            user_exit();
        return;
    }
   struct review* rev_current=malloc(sizeof(struct review));

    int j=0;
    while ( current != NULL )
    {
        if(current->id ==2)
            current=current->next;
        if(strcmp(current->name,str)==0){
        //Printing info
        printf("________________________________________\n");
        printf ("Restaurant name = %s\n\nLocation = %s\n\n", current -> name, current -> location);

        rev_current= rev_head;
        while(rev_current !=NULL)
        {
            if(rev_current->id == current->id)
                printf("\tReview of %s: %d\n\n",rev_current -> user_id,rev_current-> rev);
            rev_current=rev_current -> next;

        }
        j=0;
        while(strcmp(current->food[j],"\0"))
        {
              //printf ("Too\n");
              printf("%s",current->food[j]);
              printf(" :%d /-\n",current->price[j]);
              j++;
        }
        //Moving to the next element
        flag=1;

        }
        current = current -> next;
    }
    if(flag==0) printf("Restaurant not found\n");
    printf("Press any key to continue");
    action_key=getche();
    return;

}

void search_by_location()
{
    int flag=0;
    printf("Enter location: ");
    char str[100];
    gets(str);
    struct r *current;
    current = head;
    int i;
    char action_key;
    if(head == NULL)
    {
        printf("There are no restaurants. Press any key to continue or 'e' to exit");
        action_key = getche();
        if(action_key == 'e')
            user_exit();
        return;
    }
    struct review* rev_current=malloc(sizeof(struct review));

    int j=0;
    while ( current != NULL )
    {
        if(current->id ==2)
            current=current->next;
        if(strcmp(current->location,str)==0){
        //Printing info
        printf("________________________________________\n");
        printf ("Restaurant name = %s\n\n", current -> name);

        rev_current= rev_head;
        while(rev_current !=NULL)
        {
            if(rev_current->id == current->id)
                printf("\tReview of %s: %d\n\n",rev_current -> user_id,rev_current-> rev);
            rev_current=rev_current -> next;

        }
        j=0;
        while(strcmp(current->food[j],"\0"))
        {
              //printf ("Too\n");
              printf("%s",current->food[j]);
              printf(" :%d /-\n",current->price[j]);
              j++;
        }
        //Moving to the next element
        flag=1;

        }
        current = current -> next;
    }

    if(flag==0) printf("Location not found\n");
    printf("Press any key to continue");
    action_key=getche();
    return;

}

void add_review ()
{
    char ch;
    char rest_name[80];
    struct review* n=malloc(sizeof(struct review));

    printf("Enter your name\n");
    gets(n -> user_id);
    printf("Enter name of restaurant\n");
    gets(rest_name);

    struct r *current;
    current = head;

    while ( current != NULL && strcmp(rest_name,current -> name) )
    {
        // printf ("restaurant name = %s\n", current -> name);
        current = current -> next;

    }
    if(current == NULL)
    {
        printf("The restaurant is not in database\n");
        return;
    }
    else
    {
        printf("review of %s\n", current -> name);
        n->id=current->id;
    }
    printf("Rate the restaurant\n");
    scanf("%d",&n->rev);

    n -> next = NULL;

    if ( rev_head == NULL )
    {
        rev_head = n;
    }
    else
    {
        struct review *cur = rev_head;

        //Iterate through the linked list to find the last element
        while ( cur -> next != NULL )
        {
            cur = cur -> next;
        }

        //link the last element to the new element
        cur -> next = n;
    }

    printf("Do you want to add another review?(y/n)\n");
    ch=getche();

    if(ch=='y')
        add_review();

    return;
}
int size_r(void)
{
    int c=1;
    if(head==NULL)
        return 0;
    struct r *current = head;

    while ( current -> next != NULL )
    {
        current = current -> next;
        c++;
    }
    return c;
}
void add_r ()
{
    int i;
    int j;
    char ch;
    i=size_r();
    struct r* n=malloc(sizeof(struct r));

    printf("Enter the name of the restaurant\n");
    gets(n -> name);
    n-> id=++i;

    printf("Enter the location of the restaurant\n");
    gets(n -> location);
    printf("Enter the items available there\n");
    for(j=0;j<10;j++)
    {
        printf("Item %d\n",j+1);
        gets(n-> food[j]);
        printf("Do you want to add another item?(y/n)\n");
        ch=getche();
        if(ch=='n')
        {
            strcpy(n -> food[j+1], "\0");
             break;
        }

    }
    printf("Enter price of each item\n");
    int q;
    for(q=0;q<=j;q++)
    {
        printf("%s:",n->food[q]);
        scanf("%d", &n->price[q]);

    }


      //Setting the linking part
    n -> next = NULL;

    if ( head == NULL )
    {
        head = n;
    }
    else
    {

        struct r *current = head;

        //Iterate through the linked list to find the last element
        while ( current -> next != NULL )
        {
            current = current -> next;
        }

        //link the last element to the new element
        current -> next = n;
    }

    printf("Do you want to add another restaurant?(y/n)\n");
    ch=getche();

    if(ch=='y')
        add_r();

    if(ch=='n')
        user_exit();

    return;
}

void PRINT ()
{

    struct r *current;
    current = head;
    struct  review  *rev_current = rev_head;
    printf("\nThe list contains the following information:\n");
    int j=0;
    while ( current != NULL )
    {
    if((current-> id) ==2)
        current=current->next;
        //Printing info
    printf("________________________________________\n");
        printf ("Restaurant name = %s\n\nLocation = %s\n\n", current -> name, current -> location);

        rev_current= rev_head;
        while(rev_current !=NULL)
        {
            if(rev_current->id == current->id)
                printf("\tReview of %s: %d\n\n",rev_current -> user_id,rev_current-> rev);
            rev_current=rev_current -> next;

        }
        j=0;
        while(strcmp(current->food[j],"\0"))
        {
              //printf ("Too\n");
              printf("%s",current->food[j]);
              printf(" :%d /-\n",current->price[j]);
              j++;
        }
        //Moving to the next element
        current = current -> next;

    }

    printf("\n");

    printf("If you want to exit press 'e'\nIf you want to go to main menu, press any other key");
    char tek;
    tek=getchar();
    if(tek=='e'|| tek=='E')
        user_exit();
    else
        return;

}
void user_exit()
{
    FILE *fp;
    fp = fopen("restaurants.txt", "wb");
    if(fp == NULL)
    {
        printf("Error reading file");
        return;
    }

    struct r *current;
    current = head;
    while(current != NULL)
    {
        if(fwrite(current, sizeof(struct r), 1, fp) != 1)
        {
            printf("Error writing file");
            exit(1);
        }
        current = current -> next;
    }

    FILE *fp2;
    fp2 = fopen("review.txt", "wb");
    if(fp2 == NULL)
    {
        printf("Error reading file");
        return;
    }

    struct review *current2;
    current2 = rev_head;
    while(current2 != NULL)
    {
        if(fwrite(current2, sizeof(struct review), 1, fp2) != 1)
        {
            printf("Error writing file");
            exit(1);
        }
        current2 = current2 -> next;
    }
    exit(1);
}
void load()
{

    FILE *fp;
    fp = fopen("restaurants.txt", "rb");

    if(fp == NULL)
    {
        printf("Error reading file1\n");
        return;
    }
    struct r *current_read, *current_node;
    while(1)
    {
        current_read = malloc(sizeof(struct r));
        if(fread(current_read, sizeof(struct r), 1, fp) != 1)
        {
            if(feof(fp))
            {
                return;
            }
            printf("No restaurants to show");
            return;
        }
        // putting NULL to mark the end of list(current contact to be read will be the last node)
        // next contact to be read will be put after last node in 'else' block
        current_read -> next = 0;

        if(head == NULL)
        {
            head = current_read;
        }
        else
        {
            current_node = head;
            while(current_node -> next != NULL)
            {
                current_node = current_node -> next;
            }
            current_node -> next = current_read;
        }
    }
    return;
}
void rev_load()
{

    FILE *fp2;
    fp2 = fopen("review.txt", "rb");

    if(fp2 == NULL)
    {
        printf("Error reading file2\n");
        return;
    }
    struct review *current_read, *current_node;
    while(1)
    {
        current_read = malloc(sizeof(struct review));
        if(fread(current_read, sizeof(struct review), 1, fp2) != 1)
        {
            if(feof(fp2))
            {
                return;
            }
            printf("No restaurants to show");
            return;
        }
        // putting NULL to mark the end of list(current contact to be read will be the last node)
        // next contact to be read will be put after last node in 'else' block
        current_read -> next = 0;

        if(rev_head == NULL)
        {
            rev_head = current_read;
        }
        else
        {
            current_node = rev_head;
            while(current_node -> next != NULL)
            {
                current_node = current_node -> next;
            }
            current_node -> next = current_read;
        }
    }
    return;
}
void menu()
{
    char key;
    char arrow[] = "==>>";
    char menu_option_lower[3][100];
    char menu_option_upper[3][100];
    strcpy(menu_option_lower[0],"Add a review");
    strcpy(menu_option_lower[1],"Search restaurant");
    strcpy(menu_option_lower[2],"Exit");


    strcpy(menu_option_upper[0],"ADD A REVIEW");
    strcpy(menu_option_upper[1],"SEARCH RESTAURANT");
    strcpy(menu_option_upper[2],"EXIT");

    printf("%s\t%s\n",arrow,menu_option_upper[0]);
    printf("\t%s\n",menu_option_lower[1]);
    printf("\t%s\n",menu_option_lower[2]);

    printf("Press UP and DOWN key to navigate the menu");
    int i = 0;
    int j;
    while(1)
    {
        key = getche();
        if(key == DOWN_ARROW)
        {
            if(i == 2)
                i = 0;
            else
                i++;
        }
        else if(key == UP_ARROW)
        {
            if(i == 0)
                i = 2;
            else
                i--;
        }
        if(key == ENTER)
        {
            if(i == 0)
            {
                system("cls");
               add_review();
            }
            else if(i == 1)
            {
                system("cls");
                search();
            }
            else if(i==2){
                system("cls");
                user_exit();
            }

        }
        system("cls");
        for(j = 0; j < 3; j++)
        {
            if(j == i)
                printf("%s\t%s\n",arrow,menu_option_upper[j]);
            else
                printf("\t%s\n",menu_option_lower[j]);
        }
        printf("Press UP and DOWN key to navigate the menu");
    }
}

void manager_menu()
{
    char key;
    char arrow[] = "==>>";
    char menu_option_lower[3][100];
    char menu_option_upper[3][100];
    strcpy(menu_option_lower[0],"Add a restaurant");
    strcpy(menu_option_lower[1],"View all restaurants");
    strcpy(menu_option_lower[2],"Exit");


    strcpy(menu_option_upper[0],"ADD A RESTAURANT");
    strcpy(menu_option_upper[1],"VIEW ALL RESTAURANTS");
    strcpy(menu_option_upper[2],"EXIT");

    printf("%s\t%s\n",arrow,menu_option_upper[0]);
    printf("\t%s\n",menu_option_lower[1]);
    printf("\t%s\n",menu_option_lower[2]);

    printf("Press UP and DOWN key to navigate the menu");
    int i = 0;
    int j;
    while(1)
    {
        key = getche();
        if(key == DOWN_ARROW)
        {
            if(i == 2)
                i = 0;
            else
                i++;
        }
        else if(key == UP_ARROW)
        {
            if(i == 0)
                i = 2;
            else
                i--;
        }
        if(key == ENTER)
        {
            if(i == 0)
            {
                system("cls");
               add_r();
            }
            else if(i == 1)
            {
                system("cls");
                PRINT();
            }
            else if(i==2){
                system("cls");
                user_exit();
            }

        }
        system("cls");
        for(j = 0; j < 3; j++)
        {
            if(j == i)
                printf("%s\t%s\n",arrow,menu_option_upper[j]);
            else
                printf("\t%s\n",menu_option_lower[j]);
        }
        printf("Press UP and DOWN key to navigate the menu");
    }
}

int main()
{
    system("COLOR F5");
    system("cls");
    printf("\n\n\n\n\n\n\n\t\t\t\t\t\t\t\t    WELCOME TO FOOD KOALA");
    printf("\n\t\t\t\t\t\t\t\t============================");

    printf("\t\t\t\t\t\t\t\t\t          \n");
    printf("\t\t\t\t\t\t      ________\t\t\t\t      ________\n");
    printf("\t\t\t\t\t\t      (      )    \t\t\t      (      )\n");
    printf("\t\t\t\t\t\t    (          )  \t\t\t    (          )\n");
    printf("\t\t\t\t\t\t   (            ) \t\t\t   (            )\n");
    printf("\t\t\t\t\t\t  (              )\t\t\t  (              )\n");
    printf("\t\t\t\t\t\t   (            ) \t\t\t   (            )\n");
    printf("\t\t\t\t\t\t    (          )  \t\t\t    (          )\n");
    printf("\t\t\t\t\t\t     (         \\___________________________/          )\n");
    printf("\t\t\t\t\t\t      \\\t\t _______\t   _______\t     /\n");
    printf("\t\t\t\t\t\t       \\\t(\t)\t  (\t  )         /\n");
    printf("\t\t\t\t\t\t        \\      (\t )       (\t   )       /\n");
    printf("\t\t\t\t\t\t         \\     (_________)       (_________)      /\n");
    printf("\t\t\t\t\t\t          \\               _______\t\t /\n");
    printf("\t\t\t\t\t\t           \\\t\t /\t \\\t        /\n");
    printf("\t\t\t\t\t\t            \\\t\t|\t  |\t       /\n");
    printf("\t\t\t\t\t\t             \\\t\t|\t  |\t      /\n");
    printf("\t\t\t\t\t\t              \\\t        |\t  |\t     /\n");
    printf("\t\t\t\t\t\t               \\        |\t  |\t    /\n");
    printf("\t\t\t\t\t\t                \\       \\\t  /\t   /\n");
    printf("\t\t\t\t\t\t                 \\       \\\t /\t  /\n");
    printf("\t\t\t\t\t\t                  \\----   \\_____/    ----/\n");
    printf("\t\t\t\t\t\t                      \\______________/\n");


    printf("\n\n\n\n\t\t\t\t\t\t\t\tPress Enter to proceed!");
    if(getch()==13)
        system("cls");

    load();
    rev_load();
    //menu();
    welcome_screen();

    user_exit();
    return 0;

}
