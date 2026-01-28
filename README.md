#include<stdio.h>
#include<stdlib.h>
#include<string.h>

#define MAX 20

struct datum {
    int tag;
    int monat;
    int jahr;
};
struct angestellt{
    char name[20];
    char vorname[20];
    struct datum alter;
    struct datum eingest;
    long gehalt;
    struct angestellt *next; //verkettete Liste
};
struct angestellt *next   = NULL;
struct angestellt *anfang = NULL;

//neuer Datensatz:
void anhaengen(char *n, char *v, int at, int am, int aj,
               int eint, int einm, int einj, long g){
    struct angestellt *zeiger; /*Zeiger zum Zugriff auf einzelnen
                                Elemente der Struktur */
    if(anfang == NULL){         //if *anfang leer, erste Element uebergeben
        if((anfang =
            (struct angestellt*)malloc(sizeof(struct angestellt))) == NULL){
            fprintf(stderr, "Kein Speicherplatz vorhanden"
                   "fuer anfang\n");
            return;
        }
        strcpy(anfang->name, n);
        strcpy(anfang->vorname, v);

        anfang->alter.tag = at;
        anfang->alter.monat = am;
        anfang->alter.jahr = aj;
        anfang->eingest.tag = eint;
        anfang->eingest.monat = einm;
        anfang->eingest.jahr = einj;
        anfang->gehalt = g;
        anfang->next = NULL;        //jetzt ist 1. Element in der Liste
    }
    //Anfang=!0 -> next Element in next Liste
    else {
        zeiger = anfang;        //Zeiger auf erstes Element
        while(zeiger->next != NULL)
            zeiger = zeiger -> next;
        if((zeiger->next = (struct angestellt*)malloc(sizeof(struct angestellt)))== NULL){
            fprintf(stderr, "Kein Speicherplatz fuer das"
                   "letzte Element\n");
            return;
        }
        zeiger=zeiger->next;    //Zeiger auf neuen Speicherplatz
        strcpy(zeiger->name,n);
        strcpy(zeiger->vorname,v);

        zeiger->alter.tag = at;
        zeiger->alter.monat = am;
        zeiger->alter.jahr = aj;
        zeiger->eingest.tag = eint;
        zeiger->eingest.monat = einm;
        zeiger->eingest.jahr = einj;
        zeiger->gehalt = g;
        zeiger->next = NULL;
    }
}
void loesche (char *wen){
    struct angestellt *zeiger, *zeiger1;

    if(anfang != NULL){         //ist ein Element vorhanden?
        if(strcmp(anfang->name,wen) == 0){ //ist 1. Element das gesuchte?
            zeiger=anfang->next;
            free(anfang);
            anfang=zeiger;
        }
        else {
            zeiger=anfang;      //nicht 1. Element, also weiter suchen
            while(zeiger->next != NULL) {
                zeiger1=zeiger->next;
                if(strcmp(zeiger1->name,wen) == 0) {
                    zeiger->next=zeiger1->next;
                    free(zeiger1);
                    break;
                }
                zeiger=zeiger1;
            }
        }
    }
    else
        printf("Es sind keine Daten zum Loeschen vorhanden!!");
}
void ausgabe(void){
    struct angestellt *zeiger = anfang;

    printf("||==================================="
            "=========================||\n");
    printf("|%10cName%10c |Geburtsdatum|"
           "Eingestellt|Gehalt|\n",' ',' ');
    printf("||==================================="
            "=========================||\n");

    while(zeiger != NULL){
        printf("|%12s,%-12s|%02.%02d.%04d|"
               "%02d.%02d.%04d|%06ld\n",
               zeiger->name,zeiger->vorname,zeiger->alter.tag,
               zeiger->alter.monat,zeiger->alter.jahr,
               zeiger->eingest.tag,zeiger->eingest.monat,
               zeiger->eingest.jahr,zeiger->gehalt);
               printf("|-------------------------"
                      "---------------|\n");
            zeiger=zeiger->next;
    }
}

void sortiert_einfuegen(char *n, char *v, int at, int am, int aj,
                        int et, int em, int ej, long geh) {
    struct angestellt *zeiger, *zeiger1;
    if(anfang == NULL)
        anhaengen(n,v,at,am,aj,et,em,ej,geh);
    else {
        zeiger = anfang;
        while(zeiger != NULL && (strcmp(zeiger->name,n) < 0))
            zeiger=zeiger->next;
        if(zeiger == NULL)
            anhaengen(n,v,at,am,aj,et,em,ej,geh);
        else if(zeiger == anfang){
            anfang = (struct angestellt*)malloc(sizeof(struct angestellt));
            if(NULL == anfang){
                fprintf(stderr, "Kein Speicher\n");
                return;
            }
            strcpy(anfang->name,strtok(n, "\n"));
            strcpy(anfang->vorname,strtok(v, "\n"));

            anfang->alter.tag = at;
            anfang->alter.monat = am;
            anfang->alter.jahr = aj;
            anfang->eingest.tag = et;
            anfang->eingest.monat = em;
            anfang->eingest.jahr = ej;
            anfang->gehalt = geh;
            anfang->next = NULL;
        }
            else {
                zeiger1 = anfang;
                while(zeiger1->next != zeiger)
                    zeiger1 = zeiger1->next;
                zeiger=(struct angstellt*)malloc(sizeof(struct angestellt));
                if(NULL == zeiger) {
                    fprintf(stderr, "Kein Speicher");
                }
                strcpy(anfang->name,strtok(n, "\n"));
                strcpy(anfang->vorname,strtok(v, "\n"));

                anfang->alter.tag = at;
                anfang->alter.monat = am;
                anfang->alter.jahr = aj;
                anfang->eingest.tag = et;
                anfang->eingest.monat = em;
                anfang->eingest.jahr = ej;
                anfang->gehalt = geh;
                zeiger->next=zeiger1->next;
                zeiger1->next=zeiger;
            }
        }
    }
int eingabe(void){
    char nam[MAX], vorn[MAX];
    int atag, amon, ajahr, eintag, einmon, einjahr;
    long gehalt;
    char *ptr;

    printf("Name...................: ");
    fgets(nam, MAX, stdin);
    ptr = strrchr(nam, '\n');
    *ptr = '\0';
    printf("Vorname................: ");
    fgets(vorn, MAX, stdin);
    ptr = strrchr(vorn, '\n');
    *ptr = '\0';
    printf("Alter......(tt.mm.jjjj): ");
    scanf("%2d.%2d.%4d.",&atag,&amon,&ajahr);
    printf("Eingestellt(tt.mm.jjjj): ");
    scanf("%2d.%2d.%4d.",&eintag,&einmon,&einjahr);
    printf("Monatsgehalt...........: ");
    scanf("%ld",&gehalt);
    getchar();
    sortiert_einfuegen(nam, vorn, atag, amon, ajahr, eintag,
              einmon, einjahr, gehalt);
}
int main(void){
    int wahl;
    char dname[MAX];

    do{
        printf("\n1 : Eingabe\n");
        printf("2 : Ausgabe\n");
        printf("3 : Namen loeschen\n");
        printf("9 : Ende\n");
        printf("Ihre Wahl : ");
        scanf("%d",&wahl);
        getchar();
        switch(wahl){
        case 1: eingabe(); break;

        case 2: ausgabe(); break;

        case 3: printf("Der Name zum Loeschen: ");
                fgets(dname, MAX, stdin);
                loesche(strtok(dname, "\n")); break;

        case 9: break;
        }
    }
    while(wahl != 9);

    return EXIT_SUCCESS;
}
