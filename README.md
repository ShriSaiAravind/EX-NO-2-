## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




## Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char M[5][5];

void makeMatrix(char key[]) {
    int used[26]={0}, k=0; char t[25];
    for(int i=0; key[i]; i++){
        char c=toupper(key[i]); if(c=='J') c='I';
        if(isalpha(c) && !used[c-'A']) t[k++]=c, used[c-'A']=1;
    }
    for(char c='A'; c<='Z'; c++){
        if(c=='J') continue;
        if(!used[c-'A']) t[k++]=c;
    }
    k=0;
    for(int i=0;i<5;i++) for(int j=0;j<5;j++) M[i][j]=t[k++];
}

void pos(char c, int*r,int*c2){
    if(c=='J') c='I';
    for(int i=0;i<5;i++) for(int j=0;j<5;j++)
        if(M[i][j]==c){*r=i;*c2=j;return;}
}

void prep(char s[],char o[]){
    int k=0;
    for(int i=0; s[i]; ){
        char a=toupper(s[i]); if(!isalpha(a)){i++; continue;}
        char b='X'; int j=i+1;
        while(s[j] && !isalpha(s[j])) j++;
        if(s[j]){b=toupper(s[j]); if(b=='J') b='I';}
        if(a=='J') a='I';
        if(a==b) o[k++]=a, o[k++]='X', i++;
        else     o[k++]=a, o[k++]=b, i=j+1;
    }
    o[k]=0;
}

void crypt(char in[], char out[], int dec){
    int k=0;
    for(int i=0; in[i]; i+=2){
        int r1,c1,r2,c2;
        pos(in[i],&r1,&c1); pos(in[i+1],&r2,&c2);
        if(r1==r2){
            out[k++]=M[r1][(c1+5+(dec?-1:1))%5];
            out[k++]=M[r2][(c2+5+(dec?-1:1))%5];
        } else if(c1==c2){
            out[k++]=M[(r1+5+(dec?-1:1))%5][c1];
            out[k++]=M[(r2+5+(dec?-1:1))%5][c2];
        } else {
            out[k++]=M[r1][c2];
            out[k++]=M[r2][c1];
        }
    }
    out[k]=0;
}

int main(){
    char key[100], plain[200];
    char prepared[300], enc[300], dec[300];

    printf("Enter key: ");
    scanf("%s", key);

    printf("Enter plaintext: ");
    scanf("%s", plain);

    makeMatrix(key);
    prep(plain, prepared);
    crypt(prepared, enc, 0);
    crypt(enc, dec, 1);

    printf("\nPrepared:  %s", prepared);
    printf("\nEncrypted: %s", enc);
    printf("\nDecrypted: %s\n", dec);

    return 0;
}

```

## Output:
<img width="1620" height="551" alt="image" src="https://github.com/user-attachments/assets/42796019-aa93-4d6d-a299-89a9b5d7f98e" />

## Result:
Thus the C program to implement the Playfair Substitution technique was completed and successfully executed.
