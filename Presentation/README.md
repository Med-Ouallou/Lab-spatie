---
marp: true
theme: default
class: lead
paginate: true
backgroundColor: #ffffff
color: #5B2C6F
---

<!-- Page de garde -->
# Présentation Lab Spatie
### Gestion des rôles & permissions

**Présentée par : Mohamed Ouallou**  
**Encadré par : M. Fouad Essarraj**  
**Date : 19/02/2026**

---

# Objectif du Lab

- Comprendre le package **Spatie Laravel Permission**
- Gérer les **rôles** et les **permissions**
- Sécuriser le **backend** et le **frontend**
- Mettre en place un **contrôle d’accès flexible**

---

# C’est quoi Spatie ?

**Spatie Laravel Permission** est un package Laravel qui permet de :

- Créer des **rôles** (Admin, Editor…)
- Créer des **permissions** (create, edit, delete…)
- Associer permissions <-> rôles
- Vérifier les accès utilisateurs

👉 Gestion dynamique des autorisations via base de données.

---

# Architecture interne

## Tables principales

- `roles`
- `permissions`
- `model_has_roles`
- `model_has_permissions`
- `role_has_permissions`

---

# Relation entre les tables

User  

model_has_roles  
  
Role  
  
role_has_permissions  
  
Permission  

✔ Relation many-to-many  
✔ Vérification dynamique via DB

---

# Installation

## Installer le package

```bash
composer require spatie/laravel-permission
```

# Publier les fichiers

```bash
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```

# Exécuter les migrations

```bash
php artisan migrate
```

---

# Avantages

- Gestion centralisée

- Sécurité robuste

- Flexible et dynamique

- Compatible avec Laravel

- Utilisé en production

- Facilement extensible
---

### Exemple :

- Admin → create, edit, delete
- Editor → edit
- User → view

Relation : many-to-many

---

# Configuration du modèle User

Dans le modèle User :

```php
use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    // Trait
    use HasRoles;
}
```

- assignRole()

- givePermissionTo()

- hasRole()

- can()

---

# Création des rôles et permissions

```php
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;

Permission::create(['name' => 'edit articles']);
Role::create(['name' => 'admin']);
```

---

# Attribuer des rôles et permissions

```php
$user->assignRole('admin');
$user->givePermissionTo('edit articles');
```
---

# Vérification des permissions

Dans un Controller : 
```php
if ($user->can('edit articles')) {
    // Autorisé
}
```

---

# Vérification des permissions
## Dans une Blade

```php
@can('edit articles')
    <a href="#">Edit</a>
@endcan
```

---

# Avec Middleware

```php
Route::group(['middleware' => ['role:admin']], function () {
    // Routes protégées
});
```

---

Dans Blade
```php
@can('edit articles')
    <button>Edit</button>
@endcan

```