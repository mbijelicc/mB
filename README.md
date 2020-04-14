# mB
/*Four in a row game*/
/*You will need "saved.txt" file*/

#include <stdio.h>
#include <string.h>


void board(char matrix[][7]){
    putchar('\n');
    for(int i = 0; i < 6; i++){
        printf("\t\t\t");
        for(int j = 0; j < 7; j++){
            printf(" %c\t", matrix[i][j]);
        }
    putchar('\n');
    }
    printf("\t\t\t[1]\t[2]\t[3]\t[4]\t[5]\t[6]\t[7]\t");
    putchar('\n');
}

int condition(char matrix[][7], int x, int y, int temp, char playerChar){
    int i, j, outcome = 0, xnext, ynext;
    switch(temp){
        case 1: i = -1; j = -1; break;
        case 2: i = -1; j = 0 ; break;
        case 3: i = -1; j = 1 ; break;
        case 4: i = 0; j = -1 ; break;
        case 6: i = 0; j = 1 ; break;
        case 7: i = 1; j = -1 ; break;
        case 8: i = 1; j = 0 ; break;
        case 9: i = 1; j = 1 ; break;
    }
    xnext = x + i;
    ynext = y + j;
    
    if(xnext < 0 || xnext > 6 || ynext < 0 || ynext > 7)
        return 0;
        
    if(matrix[xnext][ynext] == playerChar){
        
        xnext = xnext + i;
        ynext = ynext + j;
        
        if(xnext < 0 || xnext > 6 || ynext < 0 || ynext > 7)
            return 0;
            
        if(matrix[xnext][ynext] == playerChar){
            outcome = 1;
            matrix[xnext][ynext] = 'Y';
            matrix[xnext - i][ynext - j] = 'Y';
            matrix[xnext - 2*i][ynext - 2*j] = 'Y';
            matrix[xnext - 3*i][ynext - 3*j] = 'Y';
        }
        else{        
            if(matrix[x - 2*i][y - 2*j] == playerChar){
                outcome = 1;
                matrix[x - i][y - j] = 'Y';
                matrix[x][y] = 'Y';
                matrix[x + i][y + j] = 'Y';
                matrix[x - 2*i][y - 2*j] = 'Y';
            }
        }
    }   
    return outcome;
}

int check_win(char matrix[][7], int n, int m, char playerChar){
    int sum = 0, temp = 0, outcome = 0;
    for(int x = n - 1; x < n + 2; x++){
        for(int y = m - 1; y < m + 2; y++){
            temp++;
            if((y < 0) || (y > 7) || (x == n && y == m) || (x < 0) || (x > 6)){
                continue;
            }
            
            if(matrix[x][y] == playerChar){
                outcome = condition(matrix, x, y, temp, playerChar);
                
                if(outcome)
                    return outcome;
            }
            
        }

    }
    return outcome;
}

int numfield(char matrix[][7]){
    int temp = 0;
    for(int i=0;i<6;i++){
        for(int j=0;j<7;j++){
            if(matrix[i][j]=='.'){
                temp++;
            }
        }
    }
    return temp;
    
    
}

int main () {
    
    int manu, player = 1,move, checkRows,back,manu2,IDfile = 0, playerfile,ID, loaded=1;
    char Xname[100], Oname[100],matrix[6][7],matrixfile[6][7],Xnamefile[100],Onamefile[100],name[100];
    FILE*saved;
    while(1){
        if(manu2 != 4 || loaded == 1){
        while(1){
            
                printf("1.Play a new game\n2.Load already saved game\n3.Exit the game\n");
                scanf("%d",&manu);
                if(manu<1||manu>3){
                    printf("You have entered unknown command\n");
                }
                else{
                    break;}
                
            }
        }
        else{
            manu=1;
        }
        if(manu == 3){
            printf("Thank You for playing the game!<3");
            return 0;
        }
        
        switch(manu){
            case 1: 
                while(1){
                    if(loaded==1 ){
                        player = 2;
                        printf("Enter name for the player X: ");
                        scanf("%s",Xname);
                        printf("\nEnter name for the player O: ");
                        scanf("%s",Oname);
                        printf("\n");
                        
                        for(int i = 0; i < 6; i++){
                            for(int j = 0; j < 7; j++){
                                matrix[i][j] = '.';
                            }
                        }
                    }
                    board(matrix);
                    
                    while(1){
                        while(1){
                            if(player == 2){
                            while(1){
                                printf("Enter the number from 1 to 7 or 0 to save\nPlayer X turn:");
                                player = 1;
                                scanf("%d",&move);
                                printf("\n");
                                if(move == 0){
                                    saved = fopen("saved.txt", "r");
                                    while(fscanf(saved, " %d %s %s %d ",&IDfile,Xnamefile,Onamefile,&playerfile) == 4){
                                        for(int i = 0; i < 6; i++){
                                            for(int j = 0; j < 7; j++){
                                                fscanf(saved,"%c ",&matrixfile[i][j]);
                                            }
                                        }
                                    }
                                    fclose(saved);
                                    saved = fopen("saved.txt", "a");
                                    fprintf(saved, "%d %s %s %d ",IDfile + 1,Xname,Oname,player);
                                    for(int i = 0; i < 6; i++){
                                        for(int j = 0; j < 7; j++){
                                            fprintf(saved,"%c ",matrix[i][j]);
                                        }
                                    }
                                    fputc('\n', saved);

                                    printf("Your game was successfully saved with a unique ID of %d\n", IDfile + 1);
                                    
                                    fclose(saved);
                                    break;
                                }else if(move<1||move>7){
                                    printf("You entered unknown command\nPlease try again\n");
                                }else if(matrix[0][move - 1] != '.'){
                                    printf("You have entered a full row.\nPlease try again\n");    
                                }
                                else{
                                    break;
                                }
                            }
                            if(move == 0)
                                break;
        
                            for(checkRows = 5; checkRows >= 0; checkRows--){
                                if( matrix[checkRows][move - 1] == '.'){
                                    matrix[checkRows][move - 1] = 'X';
                                    break;
                                }
                            }
                            
                            board(matrix);
                            if(check_win(matrix, checkRows, move - 1, 'X') == 1){
                                printf("X won!\n");
                                board(matrix);
                                while(1){
                                printf("If You want to start a new game enter 1, if You want to go to the manu 2\n");
                                scanf("%d",&back);
                                    if(back<1||back>2){
                                        printf("You entered unknown command\nPlease try again\n");
                                    }
                                    else
                                        break;
                                    
                                }
                                if(back == 1){
                                    printf("Enter name for the player X: ");
                                    scanf("%s",Xname);
                                    printf("\nEnter name for the player O: ");
                                    scanf("%s",Oname);
                                    printf("\n");
                                    
                                    for(int i = 0; i < 6; i++){
                                        for(int j = 0; j < 7; j++){
                                            matrix[i][j] = '.';
                                        }
                                    }
                                    player = 2;
                                }
                                if(back==2 || back==1){
                                    loaded = 1;
                                    break;
                                }

                            }
                        }
                        if(player == 1){
                            while(1){
                                printf("Enter the number from 1 to 7 or 0 to save\nPlayer O turn:");
                                player = 2;
                                scanf("%d",&move);
                                printf("\n");
                                if(move == 0){
                                    saved = fopen("saved.txt", "r");
                                    while(fscanf(saved, " %d %s %s %d ",&IDfile,Xnamefile,Onamefile,&playerfile) == 4){
                                        for(int i = 0; i < 6; i++){
                                            for(int j = 0; j < 7; j++){
                                                fscanf(saved,"%c ",&matrixfile[i][j]);
                                            }
                                        }
                                    }
                                    fclose(saved);
                                    saved = fopen("saved.txt", "a");
                                    fprintf(saved, "%d %s %s %d ",IDfile + 1,Xname,Oname,player);
                                    for(int i = 0; i < 6; i++){
                                        for(int j = 0; j < 7; j++){
                                            fprintf(saved,"%c ",matrix[i][j]);
                                        }
                                    }
                                    fputc('\n', saved);

                                    printf("Your game was successfully saved with a unique ID of %d\n", IDfile + 1);
                                    fclose(saved);
                                    break;
                                }else if(move<1||move>7){
                                    printf("You entered unknown command\nPlease try again\n");
                                }else if(matrix[0][move - 1] != '.'){
                                    printf("You have entered a full row.\nPlease try again\n");    
                                }
                                else{
                                    break;
                                }
                            }
                            if(move == 0)
                                break;
        
                            for(checkRows = 5; checkRows >= 0; checkRows--){
                                if( matrix[checkRows][move - 1] == '.'){
                                    matrix[checkRows][move - 1] = 'O';
                                    break;
                                }
                            }
                            
                            board(matrix);
                            if(check_win(matrix, checkRows, move - 1, 'O') == 1){
                                printf("O won!\n");
                                board(matrix);
                                while(1){
                                printf("If You want to start a new game enter 1, if You want to go to the manu 2\n");
                                scanf("%d",&back);
                                    if(back<1||back>2){
                                        printf("You entered unknown command\nPlease try again\n");
                                    }
                                    else
                                        break;
                                    
                                }
                                if(back == 1){
                                    printf("Enter name for the player X: ");
                                    scanf("%s",Xname);
                                    printf("\nEnter name for the player O: ");
                                    scanf("%s",Oname);
                                    printf("\n");
                                    
                                    for(int i = 0; i < 6; i++){
                                        for(int j = 0; j < 7; j++){
                                            matrix[i][j] = '.';
                                        }
                                    }
                                    player = 2;
                                }
                                if(back==2 || back==1){
                                    loaded = 1;
                                    break;
                                }

                            }
                            if(matrix[0][1] != '.' && matrix[0][2] != '.' && matrix[0][3] != '.' && matrix[0][4] != '.' && matrix[0][5] != '.' && matrix[0][6] != '.'){
                                printf("All collums have been filled, and there is no winner\n");
                                board(matrix);
                                while(1){
                                printf("If You want to start a new game enter 1, if You want to go to the manu 2\n");
                                scanf("%d",&back);
                                    if(back<1||back>2){
                                        printf("You entered unknown command\nPlease try again\n");
                                    }
                                    else
                                        break;
                                    
                                }
                                if(back == 1){
                                    printf("Enter name for the player X: ");
                                    scanf("%s",Xname);
                                    printf("\nEnter name for the player O: ");
                                    scanf("%s",Oname);
                                    printf("\n");
                                    
                                    for(int i = 0; i < 6; i++){
                                        for(int j = 0; j < 7; j++){
                                            matrix[i][j] = '.';
                                        }
                                    }
                                    player = 2;
                                }
                                if(back==2 || back==1){
                                    loaded = 1;
                                    break;
                                }
                            }
                        }
                    
                    
                    }
                    
                    if(back == 2 || move == 0){
                        break;
                    }
                    
     
                }
                break;
                
            case 2:
                while(1){
                    while(1){
                        printf("1.List all saved games\n2.List all saved games for a particular player\n3.Show the board of one of the saved games\n4. Load a game\n5.Return to main menu \n");
                        scanf("%d",&manu2);
                        if(manu2<1||manu2>5){
                            printf("You entered unknown value\n");
                        }else{
                            break;
                        }
                        
                    }
                    if(manu2==5){
                        break;
                    }
                    int temp = 0;
                    switch(manu2){
                        case 1:
                            saved=fopen("saved.txt","r");
                            while(fscanf(saved, " %d %s %s %d ",&IDfile,Xnamefile,Onamefile,&playerfile) == 4){
                                temp = 1;
                                for(int i = 0; i < 6; i++){
                                    for(int j = 0; j < 7; j++){
                                        fscanf(saved,"%c ",&matrixfile[i][j]);
                                    }
                                }
                                printf(" ID: %d, Player X: %s, Player O: %s, Number of free fields left on the board: %d\n",IDfile,Xnamefile,Onamefile,numfield(matrixfile));
                                
                            }
                            if(temp == 0){
                                printf("Saved.txt is empty!\n");
                            }
                            getchar();
                            getchar();
                            
                            fclose(saved);
                            break;
                            
                        case 2:
                            printf("Enter name that You would like to search for: \n");
                            scanf("%s",name);
                             saved=fopen("saved.txt","r");
                            while(fscanf(saved, " %d %s %s %d ",&IDfile,Xnamefile,Onamefile,&playerfile) == 4){
                                temp = 1;
                                for(int i = 0; i < 6; i++){
                                    for(int j = 0; j < 7; j++){
                                        fscanf(saved,"%c ",&matrixfile[i][j]);
                                    }
                                }
                                if(strcmp(name,Xnamefile) == 0 ||strcmp(name,Onamefile) == 0 ){
                                   printf(" ID: %d, Player X: %s, Player O: %s, Number of free fields left on the board: %d\n",IDfile,Xnamefile,Onamefile,numfield(matrixfile));
 
                                }

                            }
                            if(temp == 0){
                                printf("Saved.txt is empty!\n");
                            }
                            getchar();
                            getchar();
                            
                            fclose(saved);
                            break;
                            
                        case 3:
                            while(1){
                                int IDtemp = 0;
                                printf("Enter ID that You would like to search for: \n");
                                scanf("%d",&ID);
                                 saved=fopen("saved.txt","r");
                                while(fscanf(saved, " %d %s %s %d ",&IDfile,Xnamefile,Onamefile,&playerfile) == 4){
                                    temp = 1;
                                    for(int i = 0; i < 6; i++){
                                        for(int j = 0; j < 7; j++){
                                            fscanf(saved,"%c ",&matrixfile[i][j]);
                                        }
                                    }
                                    if(ID==IDfile){
                                        IDtemp = 1;
                                        board(matrixfile);
                                    }
                                    fclose(saved);
                                }
                                if(IDtemp == 0){
                                    printf("ID you have entered doesen't exist\n");
                                }else{
                                    break;
                                }
                            
                            }
                            if(temp == 0){
                                printf("Saved.txt is empty!\n");
                            }
                            getchar();
                            getchar();
                            
                            
                            break;
                            
                        case 4:
                        
                            while(1){
                                int IDtemp = 0;
                                printf("Enter ID that You would like to search for: \n");
                                scanf("%d",&ID);
                                 saved=fopen("saved.txt","r");
                                while(fscanf(saved, " %d %s %s %d ",&IDfile,Xnamefile,Onamefile,&playerfile) == 4){
                                    temp = 1;
                                    for(int i = 0; i < 6; i++){
                                        for(int j = 0; j < 7; j++){
                                            fscanf(saved,"%c ",&matrixfile[i][j]);
                                        }
                                    }
                                    if(ID==IDfile){
                                        IDtemp = 1;
                                        loaded=2;
                                        strcpy(Xname, Xnamefile);
                                        strcpy(Oname, Onamefile);
                                        
                                        if(playerfile%2){
                                            player = 2;
                                        }else
                                            player = 1;
                                        
                                        for(int i = 0; i < 6; i++){
                                            for(int j = 0; j < 7; j++){
                                                matrix[i][j]=matrixfile[i][j];
                                            }
                                        }
                                        fclose(saved);
                                        break;  
                                    }
                                    
                                }
                                fclose(saved);
                                if(IDtemp == 0){
                                    printf("ID you have entered doesen't exist\n");
                                }else{
                                    break;
                                }
                            
                                }
                            if(temp == 0){
                                printf("Saved.txt is empty!\n");
                            }
                            getchar();
                            getchar();
                            
                            
                            break;
                    }
                    if(manu2==4){
                        break;
                    }
                    break;
                }
                break;
        }
    }
}
}
