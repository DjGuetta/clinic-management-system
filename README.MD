# 🦷 Dentistry System — Sistema de Gestión Clínica Odontológica

Sistema de escritorio nativo para Windows desarrollado en **.NET 10 (WPF)** y **PostgreSQL**, diseñado para digitalizar el flujo clínico, odontograma interactivo y control financiero (presupuestos, abonos y saldos) de **Dentistry (Odontología General y Estética)**.

---

## 🏗️ Arquitectura del Sistema (3 Capas Pragmática + MVVM)

El proyecto aplica el principio **YAGNI** (sin sobreingeniería) organizado en una solución con 4 proyectos independientes:

```text
DentistrySystem/
├── src/
│   ├── Dentistry.Core/       # Dominio puro: Entidades, ValueObjects, Enums y reglas financieras
│   ├── Dentistry.Data/       # Persistencia: EF Core 10, PostgreSQL (JSONB), PDF y Backups
│   └── Dentistry.Desktop/    # UI Nativa: WPF (.NET 10), MVVM, Controles vectoriales XAML
└── tests/
    └── Dentistry.UnitTests/  # Pruebas unitarias de reglas de negocio con xUnit