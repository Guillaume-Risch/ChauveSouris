// -----------------------------------------------------------------------------
// Caracterisation d'un signal acquis relativement à un ensemble de signaux
// caracteristiques correspondant à chaque espece repertoriee de chauve-souris
// 
// version 1 : 15/05/19
//
// -----------------------------------------------------------------------------

#include <stdio.h>
#include <stdlib.h>

#define NB_ESPECES 10               // nb d'especes connues
#define MAX_ECHANTILLONS 1024       // taille max d'un tableau d'echantillonage

#define MARGELOCALE 0.4             // marge acceptable d'erreur sur un point
                                    // d'echantillonage
#define MARGEGLOBALE 0.1            // marge acceptable d'erreurs cumulees sur
                                    // la courbe
#define LONGUEUR_CHAINE 32          // taille max d'une chaine de caracteres

// -----------------------------------------------------------------------------
// Structures de donnees
// -----------------------------------------------------------------------------

typedef enum {false, true} bool;

double SignalAcquis[MAX_ECHANTILLONS];
double SignalConnu[NB_ESPECES][MAX_ECHANTILLONS];

// -----------------------------------------------------------------------------
// Fonctions utilitaires
// -----------------------------------------------------------------------------

double Abs (double valeur) {
  return (valeur<0) ? -valeur : valeur;
}

void Afficher (double SignalAcquis[MAX_ECHANTILLONS]) {
  for (unsigned int i=0; i<MAX_ECHANTILLONS; ++i)
    printf("i : %d, echantillon : %f\n", i, SignalAcquis[i]);
}

// -----------------------------------------------------------------------------
// Recuperation du signal a tester et de la banque de signaux de reference
// -----------------------------------------------------------------------------

void Init(double SignalAcquis[MAX_ECHANTILLONS]) {
  char NomFichier[LONGUEUR_CHAINE];
  FILE * FichierSignalATester;
  
  printf("Nom du fichier a tester : ");
  scanf("%s", &NomFichier);

  FichierSignalATester = fopen(NomFichier, "r");

  fread(SignalAcquis, sizeof(double), MAX_ECHANTILLONS, FichierSignalATester);

  fclose(FichierSignalATester);
}

void InitBibliotheque (double SignalConnu[NB_ESPECES][MAX_ECHANTILLONS]) {
  char NomFichier[LONGUEUR_CHAINE-1];
  FILE * Bibliotheque;
  
  printf("Nom du fichier bibliotheque : ");
  scanf("%s", &NomFichier);

  Bibliotheque = fopen(NomFichier, "r");

  fread(SignalConnu, sizeof(double), NB_ESPECES*MAX_ECHANTILLONS, Bibliotheque);

  fclose(Bibliotheque);
}

// -----------------------------------------------------------------------------
// Calcul de la distance entre les courbes
// -----------------------------------------------------------------------------

void Distance (double SignalAcquis[MAX_ECHANTILLONS],
	       double SignalConnu[NB_ESPECES][MAX_ECHANTILLONS]) {

  unsigned int Cumul;
  double Deviation, DeviationLocale;

  bool SignalReconnu=false;

  for (unsigned int e=0; e<NB_ESPECES; ++e) {
    Cumul=0;
    for (unsigned int i=0; i<MAX_ECHANTILLONS; ++i) {
      DeviationLocale=Abs(SignalAcquis[i]-SignalConnu[e][i]);
      if (DeviationLocale>MARGELOCALE) ++Cumul;
    }
    
    // pourcentage de degradation par rapport au signal de reference :
    Deviation=(double)Cumul/MAX_ECHANTILLONS;

    if (Deviation<=MARGEGLOBALE) {
      SignalReconnu=true;
      printf("Signal reconnu appartenant à l'espece %d avec un ecart de %f%%\n",e , Deviation*100);
    }
  }

  if (!SignalReconnu) printf("Signal n'appartenant a aucune espece repertoriee\n");
}

// -----------------------------------------------------------------------------
// main
// -----------------------------------------------------------------------------

int main (void) {
  Init(SignalAcquis);
  InitBibliotheque(SignalConnu);

  // Comparaison du SignalAcquis avec l'ensemble des signaux connus pour
  // chaque espece
  Distance(SignalAcquis, SignalConnu);

  return 0;
}
