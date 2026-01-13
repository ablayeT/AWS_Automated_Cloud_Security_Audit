Audit de Sécurité Cloud Automatisé (AWS)
Objectif du Projet
Ce projet démontre une approche DevSecOps appliquée à l'infrastructure cloud. L'objectif est de déployer une infrastructure via du code (IaC), de détecter automatiquement des erreurs de configuration de sécurité critiques (ici, un port SSH 22 ouvert sur tout Internet) et de valider les corrections.

Il prouve la capacité à :

Utiliser l'Infrastructure-as-Code (Terraform) pour gérer des ressources AWS.

Implémenter un pipeline CI/CD de sécurité avec GitHub Actions.

Utiliser des outils d'audit de sécurité standard de l'industrie (Prowler).

Appliquer le principe de remédiation immédiate.

Architecture et Flux de Travail
Le projet suit le cycle de vie suivant :

Déploiement (IaC) : Création d'un Security Group AWS via Terraform.

Audit Automatisé : À chaque push sur la branche main, un workflow GitHub Actions lance Prowler.

Détection : Prowler scanne la configuration et alerte si le port 22 est ouvert sur 0.0.0.0/0.

Remédiation : Modification du code Terraform pour restreindre l'accès à une IP admin unique.

(Insérer ici un schéma simple si tu en as un, sinon supprimer cette ligne)

 Technologies Utilisées
AWS (EC2 / VPC Security Groups) : Cible de l'infrastructure.

Terraform : Provisionnement de l'infrastructure (IaC).

Prowler : Outil open-source de sécurité pour les audits de bonnes pratiques AWS et conformité (CIS, etc.).

GitHub Actions : Orchestration du pipeline d'audit continu.

Scénario de Sécurité : La Faille SSH
1. État Initial (Vulnérable)
L'infrastructure a été initialement déployée avec une règle ingress permissive, exposant le port SSH au monde entier. C'est une mauvaise configuration critique (Risque élevé).

Terraform

# Code vulnérable (Simulé)
ingress {
  from_port   = 22
  to_port     = 22
  cidr_blocks = ["0.0.0.0/0"] #  DANGER : Ouvert à tous
}
2. Détection par Prowler
Le pipeline CI/CD a exécuté le check ec2_securitygroup_allow_ingress_from_internet_to_tcp_port_22.

[PLACEHOLDER] :

3. Correction (Sécurisé)
Le code a été corrigé pour n'autoriser que l'IP de l'administrateur.

Terraform

# Code corrigé (Actuel)
ingress {
  description = "SSH restreint a une adresse IP interne (Admin)"
  from_port   = 22
  to_port     = 22
  cidr_blocks = ["10.0.0.5/32"] # SÉCURISÉ
}
[PLACEHOLDER] : 

Comment reproduire ce projet
Prérequis
Un compte AWS actif.

Un compte GitHub.

Terraform installé localement (pour les tests manuels).

Installation
Cloner le dépôt :

Bash

git clone https://github.com/ton-pseudo/ton-repo-audit-aws.git
cd ton-repo-audit-aws
Configurer les Secrets GitHub : Dans les paramètres du dépôt (Settings > Secrets and variables > Actions), ajoutez :

AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

Lancer l'audit : Effectuez un commit et un push sur la branche main. L'onglet "Actions" de GitHub affichera le déroulement du scan Prowler.

Projet réalisé dans le cadre de ma montée en compétence en Cybersécurité Cloud.
