```mermaid
%%{init: {'themeVariables': {'activationBkgColor': 'transparent', 'activationBorderColor': '#000000'}}}%%
sequenceDiagram
    actor Utilisateur
    participant Logiciel
    participant AppliPaiement as Appli de Paiement
 
    activate Utilisateur
    activate Logiciel
 
    Break Retry
        Utilisateur->>Logiciel: Connexion via agent
        Utilisateur->>Logiciel: Connexion via connexion
        Logiciel-->>Utilisateur: Demande Identifiant / Ressayer
    end
 
    Logiciel-->>Utilisateur: Choix des chambres et date/heure possible
    Break APT Choix Date    
        Utilisateur->>Logiciel: J+8 Paiement Arrhes
        Utilisateur->>Logiciel: J-8 Paiement confirmation
        
    end
    Utilisateur->>Logiciel: Sélection des Choix
 
    Logiciel-->>Utilisateur: Requête Saisie ID Paiement
    
    Utilisateur->>Logiciel: Saisie Coordonnées Bancaires
    Logiciel->>AppliPaiement: Demande Confirmation paiement
    activate AppliPaiement
    AppliPaiement-->>Logiciel: Acceptation
    deactivate AppliPaiement
    Logiciel->>Utilisateur: Envoi Email confirmation
    
    
    
 
    deactivate Logiciel
    deactivate Utilisateur
```
