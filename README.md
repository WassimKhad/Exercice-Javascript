edux est un design pattern et une architecture pattern.

Redux appartient à la famille d'architecture unidirectionnelle et se base sur Flux.

Flux is the application architecture that Facebook uses for building client-side web applications. It complements React's composable view components by utilizing a unidirectional data flow. It's more of a pattern rather than a formal framework.

Redux est crée pour :

organiser le flow de communication dans l'application (un seul sens de communication).
centraliser la gestion de l'état dans une application (single source of truth).
absorber la complexité d'une application grâce à la composition (scalabilité et isolation).
si la gestion de l'état est centralisée alors elle peut être facilement debugger (Redux Devtools).
rendre l'état de l'application prédictible ( état application = état du store Redux) pour faciliter les tests unitaires.
Redux est facilement extensible grâce aux middlewares (on peut ajouter des middlewares existants comme Saga ou créer nos propores middlewares et les attacher à Redux).
Reduxify
L'implémentation Redux dans les AWTs se repose sur Reduxify.

Reduxify est une façon pour appeler Redux via un décorateur au lieu de la façon ordinaire via mapStateToProps et mapDispatchToProps.

Pourquoi ? Pour éviter la réecriture des states et des fonctions dans mapStateToProps et mapDispatchToProps et créer plutôt des parties (states et dispatchers) réutilisables comme décrit ci-dessous.

mapStateToProps => Reduxify
Dans l'exemple ci-dessous, au lieu de réécrire le même code à chaque fois où on a besoin du state correspondant aux habilités virement (au niveau de mapStateToProps), on crée un fragment VirementHabilitesFragement  réutilisable que l'on appelle au besoin et dans plusieurs endroits sans répéter le code (via Reduxify):

mapStateToProps (Redux)
// retrieve props from redux
const mapStateToProps = (state) => {
  const {
    virEuroSouscris,
    virIntSouscris,
    virBonPayerSouscris,
    virTresorieSouscris,
  } = state;
  const {
    isVirEuroSouscris,
    loading: isVirEuroSouscrisLoading,
  } = virEuroSouscris || {};
  const {
    isVirBonPayerSouscris,
    loading: isVirBonPayerSouscrisLoading,
  } = virBonPayerSouscris || {};
    const {
    isVirIntSouscris,
    loading: isVirIntSouscrisLoading,
  } = virIntSouscris || {};
  const {
    isVirTresorieSouscris,
    loading: isVirTresorieSouscrisLoading,
  } = virTresorieSouscris || {};
Reduxify
VirementHabilitesFragement contient le même code répété avant dans mapStateToProps :

const VirementHabilitesFragement = state => ({
  isVirTresorieSouscris: state?.virTresorieSouscris?.isVirTresorieSouscris,
  isVirTresorieSouscrisLoading: state?.virTresorieSouscris?.loading,
  ...
mapDispatchToProps => Reduxify
La même chose pour mapDispatchToProps, au lieu de réécrire le même code à chaque fois où on a besoin du même dispatcher (au niveau de mapDispatchToProps), on crée un VirementDispatcher réutilisable que l'on appelle au besoin et dans plusieurs endroits sans répéter le code (via Reduxify):

mapDispatchToProps (Redux)
// dispatch action to redux saga
const mapDispatchToProps = dispatch => ({
  requestViperSouscrisStatus: () => dispatch({
    type: ViperActions.VIPER_IS_SOUSCRIS_REQUEST,
  }),
  requestIsHabilitationSaisie: () => dispatch({
    type: ViperActions.VIPER_IS_HABILITATION_SAISIE_REQUEST,
  }),
  ...
Reduxify
VirementDispatcher contient le même code répété avant dans mapDispatchToProps.

💡 L'idée derrière Reduxify est de minimiser le code écrit et la répétition en créant des parties réutilisables.

Le code source Reduxify peut-être consulté ici ici.

RequestManager
Le module RequestManager du package awt-react-extras permet de lancer la requête HTPP (async/await ou Promise) ensuite de parser la réponse HTTP selon le type :

ODT : {commun: {statut: 'OK', raison: null} , donnes: ...}
Default: comme la réponse par défaut d'axios Plus de détails.
Quelques soit le type (ODT ou Default), le retour du RequestManager est toujours de type :

en cas du succès: {status: number, data: any, error: null}
en cas d'erreur : {status: number, data: any, error: any}
Plus d'informations sur le RequestManager

⚠️ Le module ResponseFormatter est devenu deprecated et il est remplacé par RequestManager.

Le flow Redux
REDUX_13.png

Les composants principales de Redux sont :

Les actions (les événements)
Le reducer.
Le dispatcher.
Le store.
Le store est équivalent à un state globale partagé entre les différents composants.

Pour accèder au store, on doit soumettre une demande (une action) au Reducer :

Javascript
Exemple Complet

const GET_VERSION_DATA_REQUEST = 'GET_VERSION_DATA_REQUEST';
const GET_VERSION_DATA_REQUEST_SUCCESS = 'GET_VERSION_DATA_REQUEST_SUCCESS';
const GET_VERSION_DATA_REQUEST_FAILURE = 'GET_VERSION_DATA_REQUEST_FAILURE';

const ExempleActions = {
  GET_VERSION_DATA_REQUEST,
  GET_VERSION_DATA_REQUEST_SUCCESS,
  GET_VERSION_DATA_REQUEST_FAILURE,
};

export default ExempleActions;
Typescript

Javascript
Exemple Complet

import ExempleActions from './ExempleActions';

const {
  GET_VERSION_DATA_REQUEST,
} = ExempleActions;

const requestVersionData = (fakeParam) => ({
  type: GET_VERSION_DATA_REQUEST,
  payload: {
    fakeParam,
  },
});

const ExempleDispatcher = {
  requestVersionData,
};

export default ExempleDispatcher;
Typescript

Le Reducer est le seul décideur du nouvel état :

Javascript
Exemple Complet

import {
  Matcher,
} from 'awt-react-extras';
import ExempleActions from './ExempleActions';

const {
  GET_VERSION_DATA_REQUEST,
  GET_VERSION_DATA_REQUEST_SUCCESS,
  GET_VERSION_DATA_REQUEST_FAILURE,
} = ExempleActions;

// reducer initial state
const initialState = {
  loading: false,
  versionData: null,
  error: null,
};

const ExempleVersionDataReducer = (state = initialState, action = null) => Matcher()
  .on(() => action.type === GET_VERSION_DATA_REQUEST,
    () => ({
      ...state,
      loading: true,
      versionData: null,
      error: null,
    }))
  .on(() => action.type === GET_VERSION_DATA_REQUEST_SUCCESS,
    () => ({
      ...state,
      loading: false,
      versionData: action.versionData,
      error: null,
    }))
  .on(() => action.type === GET_VERSION_DATA_REQUEST_FAILURE,
    () => ({
      ...state,
      loading: false,
      versionData: null,
      error: action.error,
    }))
  .otherwise(() => state);

export default ExempleVersionDataReducer;
Typescript

Tous les changements d'état se font uniquement au niveau du Reducer (single source of truth).

Pour que l'architecture soit flexible, scalable et moins complexe, Redux repose sur la technique de la composition :

au lieu d'avoir un seul Reducer et un seul état : on compose des petits Reducers.
chaque sous état peut être considéré comme une Table SQL ou un Model ou une Entity.
REDUX_15.png

import { combineReducers, } from 'redux';

const ReduxReducers = reducers => combineReducers(reducers);

export default ReduxReducers;
Une fois l'état est mis à jour, le nouveau état est injecté dans la page grâce au StateProvider :

Javascript
Exemple Complet

const AwtContextFragment = (state) => ({
  configuration: state?.awtContext?.awtContext,
});

const ExempleVersionDataFragment = (state) => ({
  versionData: state?.ExempleVersionData?.versionData,
  versionDataError: state?.ExempleVersionData?.error,
  versionDataLoading: state?.ExempleVersionData?.loading,
});

const ExempleProvider = {
  AwtContextFragment,
  ExempleVersionDataFragment,
};

export default ExempleProvider;
Typescript

L'état Redux est transformée en props dans la page :

Javascript
Exemple Complet

...
  render() {
    const {
      configuration,
      versionData,
      versionDataError,
      versionDataLoading,
    } = this.props;

    commonLog.info('[ExempleVersionDataPage] awtContext : ', configuration);

    return (
        <ExempleVersionDataView
          versionData={versionData}
          versionDataError={versionDataError}
          versionDataLoading={versionDataLoading}
        />
    );
  }
}
Typescript

Pour connecter, une page à Redux, on utilise le décorateur Reduxify :

Javascript
Exemple Complet

const {
  AwtContextFragment,
  ExempleVersionDataFragment,
} = ExempleProvider;

@Reduxify((state) => ({
  // define state to extract
  ...AwtContextFragment(state),
  ...ExempleVersionDataFragment(state),
}), {
  // define actions to execute
  ...ExempleDispatcher,
})
class ExempleVersionDataPage extends Component 
Typescript

Reduxify connecte la page aux Reducers et Dispatcher auxquels elle s'intêresse.

Le workflow Redux peut être ainsi résumer dans ce schéma :

REDUX_16.png

REDUX_17.png

Les principes de Redux
Single source of truth.
Composition.
Le state dans un Reducer doit être readonly (immutable et prédictible => testable).
Les Reducers doivent être des functions pures (on fournie un nouveau état à chaque fois => avoir un timeline pour tracer comment l'état a changé => debug simple).
Les fragments du state doivent être réutilisables.
Les actions sont aussi réutilisables.
Saga
Saga pattern
SAGA PATTERN

A saga is a sequence of transactions that updates each service and publishes a message or event to trigger the next transaction step. A transaction is a single unit of logic or work, sometimes made up of multiple operations. Within a transaction, an event is a state change that occurs to an entity, and a command encapsulates all information needed to perform an action or trigger a later event. Transactions must be atomic, consistent, isolated, and durable (ACID). Transactions within a single service are ACID, but cross-service data consistency requires a cross-service transaction management strategy.

En se basant sur cette définition, dans un frontend React-Redux-Saga, Saga s'occupe de lancer les transactions HTTP et d'envoyer un acquittement (une action) mettant en évidence le statut de la transaction Fail ou Success :

Javascript
Exemple Complet

import { call, put, } from 'redux-saga/effects';
import commonLog from 'awt-common-log';
import {
  getBaseUrl,
  ResponseType,
  RequestManager,
} from 'awt-react-extras';
import { ExempleActions, } from '../redux';
import ExempleApi from './ExempleApi';
import ExempleServicesConstants from './ExempleServicesConstants';

const {
  GET_VERSION_DATA_REQUEST_SUCCESS,
  GET_VERSION_DATA_REQUEST_FAILURE,
} = ExempleActions;

const {
  getVersionDataQuery,
} = ExempleApi;

const {
  GET_VERSION_DATA_SERVICE_PATH,
} = ExempleServicesConstants;

// worker => it must be a generator
export default function* getVersionData(action) {
  // retrieve awtContext
  // from redux store
  const baseUrl = yield call(getBaseUrl);

  // read query params
  const params = action.payload;

  // execute & parse query
  /*
  For Default response parser you can do :
  const {
    error,
    data,
  } = yield call(
      RequestManager,
      getVersionDataQuery,
      ResponseType.DEFAULT,
      baseUrl,
      params
    );
  */
  const {
    error,
    data,
  } = yield call(
    RequestManager,
    getVersionDataQuery,
    ResponseType.ODT,
    baseUrl,
    params
  );

  // inform UI with request status
  if (error) {
    commonLog.error(`[ExempleVersionDataService] Appel du service ${GET_VERSION_DATA_SERVICE_PATH} KO`, error);

    // dispatch a failure action
    // to the store with the error
    yield put({
      type: GET_VERSION_DATA_REQUEST_FAILURE,
      error,
    });
  } else {
    // dispatch a success action
    // to the store with the list
    yield put({
      type: GET_VERSION_DATA_REQUEST_SUCCESS,
      versionData: data,
    });
  }
}
Typescript

Saga isole tous les side effects HTTP et asynchrones de la partie UI.

Redux & Saga
Vue que le pattern Saga est basé lui même sur le mécanisme d'événements, il s'interface simplement et fluidement avec Redux. Pour entrer dans l'ecosystème Redux, la clé est d'envoyer une action.

REDUX_18.png

Le Reducer est à l'écoute des événements émis par chaque service Saga pour mettre à jour l'état selon le cas :

  .on(() => action.type === GET_VERSION_DATA_REQUEST_SUCCESS,
    () => ({
      ...state,
      loading: false,
      versionData: action.versionData,
      error: null,
    }))
  .on(() => action.type === GET_VERSION_DATA_REQUEST_FAILURE,
    () => ({
      ...state,
      loading: false,
      versionData: null,
      error: action.error,
    }))
Saga est un middleware Redux, ce qui lui permet d'écouter et intercepter les événements envoyer vers le store Redux et exécuter ces propres traitements :

import { takeLatest, } from 'redux-saga/effects';
import { ExempleActions, } from '../redux';
import ExempleVersionDataService from './ExempleVersionDataService';
import UnAutreService from './UnAutreService';

const ExempleServicesRoot = [
  takeLatest(ExempleActions.GET_VERSION_DATA_REQUEST,
    ExempleVersionDataService),
  takeLatest(ExempleActions.GET_UN_AUTRE_SERVICE_REQUEST,
    UnAutreService),
];

export default ExempleServicesRoot;
Saga aussi se repose sur la technique de la Composition pour assurer la flexibilité et absorber la complexité et les évolutions :

// add here all your saga service root
import { all, } from 'redux-saga/effects';
import ExempleServicesRoot from '../../features/exemple/services/ExempleServicesRoot'; // feature exemple
import CreditServicesRoot from '../../features/credit/services/CreditServicesRoot'; // feature credit

// add here all your saga service root
export default function* rootSaga() {
  yield all([
    ...ExempleServicesRoot,
    ...CreditServicesRoot,
  ]);
}
Architecture finale
![image](https://github.com/user-attachments/assets/2c448e2e-eabc-4557-a6e4-2bc748f1fd1a)
