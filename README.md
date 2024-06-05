# Exercice-Javascript

Modern_Javascript_exercice_cartes
const cartesList = [
{
  numeroCarteLastDigits: '0983',
    aliasCarte: 'A090207863Crm5p8nCDX1lgqlt000',
    codeTypeDebit: '1',
    typeCarte: {
      id: 'visa_alterna',
      libelle: 'CB Visa Alterna',
    },
    idStatut: 'active',
    numeroCompteLastDigits: '9598',
    codeVisuel: null,
    collection: false,
    cryptoDynamique: false,
    grandVoyageur: false,
  },
  {
    numeroCarteLastDigits: '0999',
    aliasCarte: 'A090211626Crm5pPnKDTjegqZR000',
    codeTypeDebit: '2',
    typeCarte: {
      id: 'visa_classique',
      libelle: 'CB Visa',
    },
    idStatut: 'active',
    numeroCompteLastDigits: '9598',
    codeVisuel: '163',
    collection: true,
    cryptoDynamique: false,
    grandVoyageur: false,
  },
  {
    numeroCarteLastDigits: '4990',
    aliasCarte: 'A090211626Crm5pPnK9Xs3tqZI000',
    codeTypeDebit: '2',
    typeCarte: {
      id: 'visa_classique',
      libelle: 'CB Visa',
    },
    idStatut: 'opposition',
    numeroCompteLastDigits: '9598',
    codeVisuel: '163',
    collection: true,
    cryptoDynamique: false,
    grandVoyageur: false,
  },
];

// Afficher dans la console :
// 1- toutes les cartes ayant l'option collection active
// 2- afficher un message 'Vous avez x cartes actives' avec x = nb cartes actives
// 3- afficher un message 'Vous avez au moins une carte avec l'option grand voyageur' ou 'Vous n'avez aucune carte avec l'option grand voyageur'
// 4- afficher un message 'Toutes vos cartes sont actives' ou 'Vous avez au moins une carte en opposition'
