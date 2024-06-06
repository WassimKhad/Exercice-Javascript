Correction 
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

// 1- cartes ayant l'option collection active
const cardsWithCollectionOption = (cartesList || [])
  .filter(carte => carte.collection);
console.log('cardsWithCollectionOption : ', cardsWithCollectionOption);

const nbActiveCardsFilter = (cartesList || [])
  .filter(carte => carte.idStatut === 'active')
  .length || 0;

// 2- message 'Vous avez x cartes actives' avec x = nb cartes actives (avec filter)
const nbActiveCardsFilterLabel = `Vous avez ${nbActiveCardsFilter} cartes actives`;
console.log('nbActiveCardsFilterLabel : ', nbActiveCardsFilterLabel);

// 2- message 'Vous avez x cartes actives' avec x = nb cartes actives (avec reduce)
const nbActiveCardsReduce = (cartesList || [])
  .reduce((acc, item) => acc + (item.idStatut === 'active' ? 1 : 0), 0);

const nbActiveCardsReduceLabel = `Vous avez ${nbActiveCardsReduce} cartes actives`;
console.log('nbActiveCardsReduceLabel : ', nbActiveCardsReduceLabel);

// 3- message 'Vous avez au moins une carte avec l'option grand voyageur' ou 'Vous n'avez aucune carte avec l'option grand voyageur'
const hasCardWithOptionVoyageur = (cartesList || []).some(item => item.grandVoyageur);
const hasCardWithOptionVoyageurLabel = hasCardWithOptionVoyageur
  ? 'Vous avez au moins une carte avec l\'option grand voyageur'
  : 'Vous n\'avez aucune carte avec l\'option grand voyageur';
console.log('hasCardWithOptionVoyageurLabel : ', hasCardWithOptionVoyageurLabel);

// 4- message 'Toutes vos cartes sont actives' ou 'Vous avez au moins une carte en opposition'
const isAllCardsActive = (cartesList || []).every(item => item.idStatut === 'active');
const isAllCardsActiveLabel = isAllCardsActive
  ? 'Toutes vos cartes sont actives'
  : 'Vous avez au moins une carte en opposition'
console.log('isAllCardsActiveLabel : ', isAllCardsActiveLabel);
