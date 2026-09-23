``` mermaid
usecase-beta
direction LR

%% Acteurs
actor Dem("Demandeur de réservation") <<abstrait>>
actor Client("Client")
actor Agent("Agent de voyage partenaire")
actor Recep("Réceptionniste")
actor Serveur("Serveur restaurant / bar")
actor Gerant("Gérant")
actor Temps("Temps (horloge)") <<temps>>
actor Pay("Service de paiement externe") <<système externe>>

%% Frontière du système
systemBoundary sys["Logiciel de gestion des hôtels"]
  UC_Dispo("Consulter les disponibilités")
  UC_Reserver("Réserver une chambre")
  UC_Arrhes("Verser les arrhes")
  UC_Annuler("Annuler une réservation")
  UC_Rembourser("Rembourser le client")
  UC_AutoAnnul("Annuler automatiquement les réservations non confirmées à J-8")
  UC_Arrivee("Enregistrer l'arrivée")
  UC_Conso("Enregistrer une consommation")
  UC_Facturer("Facturer le départ")
  UC_Encaisser("Encaisser un paiement")
  UC_Arrivees("Éditer les arrivées prévues du jour")
  UC_Taux("Consulter le taux d'occupation par catégorie")
  UC_Parc("Gérer hôtels, catégories et chambres")
  UC_Tarifs("Gérer les tarifs")
end

%% Généralisation d'acteurs
Client --|> Dem
Agent --|> Dem

%% Réservation
Dem -- UC_Dispo
Dem -- UC_Reserver
Dem -- UC_Annuler
UC_Reserver ..> : include UC_Dispo
UC_Arrhes ..> : extend UC_Reserver
UC_Arrhes ..> : include UC_Encaisser
UC_Rembourser ..> : extend UC_Annuler
note for UC_Arrhes "Condition : arrivée à plus de 8 jours"
note for UC_Rembourser "Condition : remboursement dû selon le délai"

%% Annulation automatique et édition quotidienne (déclencheur : le temps)
Temps -- UC_AutoAnnul
Temps -- UC_Arrivees
Recep -- UC_Arrivees

%% Séjour
Recep -- UC_Arrivee
Recep -- UC_Conso
Serveur -- UC_Conso
Recep -- UC_Facturer
UC_Facturer ..> : include UC_Encaisser

%% Pilotage
Gerant -- UC_Taux
Gerant -- UC_Parc
Gerant -- UC_Tarifs


Pay -- UC_Encaisser
Pay -- UC_Rembourser
```
