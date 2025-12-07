# 🎨 Atriom Portfolio - Design Specifications

Ce document décrit les spécifications de design pour le site portfolio d'Atriom.

---

## 📐 Design System

### 🎨 Palette de Couleurs

| Couleur       | Hex Code  | Usage                          |
|---------------|-----------|--------------------------------|
| Primary       | `#1E3A8A` | Deep Blue - Boutons principaux, titres |
| Secondary     | `#3B82F6` | Bright Blue - Liens, accents   |
| Accent        | `#F59E0B` | Orange - CTAs, éléments importants |
| Background    | `#F9FAFB` | Light Gray - Arrière-plan      |
| Text          | `#1F2937` | Dark Gray - Texte principal    |

### ✍️ Typographie

#### Polices
- **Headings (Titres):** `'Poppins', sans-serif`
- **Body (Corps de texte):** `'Inter', sans-serif`

#### Tailles de Police
```css
h1 { font-size: 3rem; }      /* 48px */
h2 { font-size: 2.5rem; }    /* 40px */
h3 { font-size: 2rem; }      /* 32px */
body { font-size: 1rem; }    /* 16px */
```

### 📏 Espacements

Utiliser des espacements cohérents basés sur une échelle de 8px :

- **Petits espaces:** `8px`
- **Espaces moyens:** `16px`
- **Espaces standards:** `24px`
- **Grands espaces:** `32px`
- **Très grands espaces:** `48px`
- **Espaces de section:** `64px`

### 📦 Layout

- **Container max-width:** `1200px`
- **Section padding:** `64px 0`
- **Card padding:** `24px`
- **Border radius:** `8px` (standard), `16px` (large)

---

## 📱 Responsive Design

### Breakpoints

```css
/* Mobile First Approach */

/* Mobile: 320px - 767px */
/* Styles de base */

/* Tablet: 768px - 1023px */
@media (min-width: 768px) {
  /* Styles tablette */
}

/* Desktop: 1024px+ */
@media (min-width: 1024px) {
  /* Styles desktop */
}
```

### Grid System

- **Mobile:** 1 colonne
- **Tablet:** 2 colonnes
- **Desktop:** 3-4 colonnes (selon la section)

---

## 🧩 Sections du Portfolio

### 1. Hero Section
**Développeurs:** Abderrahmane (HTML) + Hasnae (CSS)

**Éléments:**
- Titre principal (H1)
- Sous-titre/tagline
- Call-to-action (CTA) button
- Image/illustration d'arrière-plan

**Design:**
- Full viewport height
- Centré verticalement et horizontalement
- Gradient background ou image hero

---

### 2. Services Section
**Développeurs:** Hasnae (HTML) + Ousama (CSS)

**Éléments:**
- Titre de section (H2)
- Grid de cartes de services
- Icônes pour chaque service
- Description courte

**Design:**
- Grid responsive (1/2/3 colonnes)
- Cards avec hover effect
- Icônes alignées et cohérentes

---

### 3. Technologies Section
**Développeurs:** Ousama (HTML) + Yasmine (CSS)

**Éléments:**
- Titre de section (H2)
- Logos/icônes des technologies
- Éventuellement niveau de compétence

**Design:**
- Grid ou flex layout
- Animation au hover
- Logos en niveaux de gris avec couleur au hover

---

### 4. Projects Section
**Développeurs:** Yasmine (HTML) + Salma (CSS)

**Éléments:**
- Titre de section (H2)
- Cards de projets
- Image de projet
- Titre et description
- Technologies utilisées
- Liens (GitHub, Demo)

**Design:**
- Grid responsive
- Image overlay au hover
- Tags pour les technologies

---

### 5. Reviews Section
**Développeurs:** Salma (HTML) + Abderrahmane (CSS)

**Éléments:**
- Titre de section (H2)
- Témoignages clients
- Photo/avatar du client
- Nom et poste
- Note (étoiles)
- Citation

**Design:**
- Carousel ou grid
- Cards élégantes avec citation
- Étoiles de notation visibles

---

### 6. Contact Section
**Développeurs:** Ousama (HTML) + Hasnae (CSS)

**Éléments:**
- Titre de section (H2)
- Formulaire de contact (nom, email, message)
- Informations de contact
- Liens réseaux sociaux
- Icônes sociales

**Design:**
- Layout 2 colonnes (desktop)
- Formulaire avec validation visuelle
- Bouton submit mis en évidence

---

## 🎯 Composants Réutilisables

### Buttons

```css
.btn {
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  transition: all 0.3s ease;
}

.btn--primary {
  background: #1E3A8A;
  color: white;
}

.btn--accent {
  background: #F59E0B;
  color: white;
}
```

### Cards

```css
.card {
  background: white;
  border-radius: 8px;
  padding: 24px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 16px rgba(0,0,0,0.15);
}
```

---

## 🎨 Convention de Nommage CSS (BEM)

Utiliser la méthodologie **BEM** (Block Element Modifier) :

```css
.block { }
.block__element { }
.block__element--modifier { }
```

**Exemple :**
```html
<section class="services">
  <div class="services__container">
    <h2 class="services__title">Our Services</h2>
    <div class="services__grid">
      <div class="services__card services__card--featured">
        <!-- Content -->
      </div>
    </div>
  </div>
</section>
```

---

## ♿ Accessibilité

- **Contraste:** Minimum 4.5:1 pour le texte normal
- **Alt text:** Toutes les images doivent avoir un attribut alt descriptif
- **Navigation clavier:** Tous les éléments interactifs accessibles au clavier
- **Focus visible:** États de focus clairement visibles
- **Hiérarchie des titres:** Respecter H1 → H2 → H3
- **HTML sémantique:** Utiliser `<header>`, `<section>`, `<article>`, `<nav>`, etc.

---

## 🎭 Animations & Interactions

### Transitions Standard
```css
transition: all 0.3s ease;
```

### Hover Effects
- **Boutons:** Changement de couleur + légère élévation
- **Cards:** translateY(-4px) + shadow augmentée
- **Liens:** Underline ou changement de couleur

### Loading States
- Skeleton screens pour le chargement
- Spinners pour les actions asynchrones

---

## 📸 Assets

### Images
- **Format:** WebP avec fallback JPG/PNG
- **Optimisation:** Compression sans perte de qualité visible
- **Sizes:** Responsive images avec srcset
- **Alt text:** Obligatoire et descriptif

### Icônes
- **Bibliothèque:** Font Awesome, Feather Icons ou SVG custom
- **Format SVG:** Pour les icônes personnalisées
- **Taille:** Cohérente dans toute l'application (généralement 24px)

---

## 🔍 SEO & Performance

- Balises meta appropriées (title, description)
- Images optimisées et compressées
- CSS minifié en production
- Lazy loading pour les images below the fold
- Semantic HTML5
- Heading hierarchy respectée

---

## ✅ Checklist Design

Avant de valider une section, vérifier :

- [ ] Respect de la palette de couleurs
- [ ] Typographie correcte (Poppins/Inter)
- [ ] Espacements cohérents (multiples de 8px)
- [ ] Responsive sur mobile/tablet/desktop
- [ ] Convention BEM respectée
- [ ] Hover states implémentés
- [ ] Accessibilité vérifiée
- [ ] Images optimisées
- [ ] Code propre et commenté

---

**Version:** 1.0  
**Dernière mise à jour:** 6 novembre 2025  
**Équipe:** Atriom Team 🚀

## Responsive Design
- **Mobile**: < 768px
- **Tablette**: 768px - 1024px
- **Desktop**: > 1024px

## Animations
- Fade-in au scroll
- Transitions fluides sur les hovers
- Animation du menu mobile