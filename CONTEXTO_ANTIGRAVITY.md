# CONTEXTO DE ARQUITECTURA Y REGLAS DEL PROYECTO: SISTEMA ODONTOLÓGICO "DENTISTRY"

## 1. Rol del Asistente en Antigravity
Actúa como un Arquitecto de Software Senior y Mentor práctico en .NET. Estoy desarrollando este sistema como único programador (Solo Fullstack) mientras aprendo desarrollo de escritorio nativo en WPF y PostgreSQL (ya tengo bases en C#, .NET, SQL, MVVM y xUnit). 
- Explícame brevemente el "porqué" de cada bloque importante que generes.
- **REGLA DE ORO (YAGNI):** Prohibida la sobreingeniería. No implementes Microservicios, MediatR, CQRS, AutoMapper, ni repositorios genéricos (`IRepository<T>`) sobre Entity Framework Core. Mantén el código simple, limpio y directo.

## 2. Stack Tecnológico y Licenciamiento (100% Open Source / Permisivo)
- **Framework:** .NET 10 (LTS) con C# 14.
- **Interfaz de Usuario (UI):** WPF Nativo (Windows Presentation Foundation) usando el patrón MVVM.
- **Base de Datos:** PostgreSQL (local/LAN).
- **ORM:** Entity Framework Core 10 (`Npgsql.EntityFrameworkCore.PostgreSQL`).
- **Paquetes NuGet autorizados (Licencias MIT / Apache 2.0 / BSD):**
  - `CommunityToolkit.Mvvm` (para `[ObservableProperty]` y `[RelayCommand]`).
  - `Microsoft.Extensions.DependencyInjection` y `Microsoft.Extensions.Hosting`.
  - `WPF-UI` (Lepoco) o `MaterialDesignThemes` para estilos modernos.
  - `PDFsharp-MigraDoc` (v6+) para exportar la historia clínica y el consentimiento a PDF.
  - `xunit` para pruebas unitarias (usar `Assert` nativos, NO usar FluentAssertions v8+).

## 3. Estructura de la Solución (Arquitectura en 3 Capas Pragmática)
La solución `DentistrySystem.sln` tiene exactamente 4 proyectos:
1. `src/Dentistry.Core` (Class Library): Entidades, ValueObjects, Enums y lógica de negocio pura (cálculo de presupuestos, abonos, saldos y alertas médicas). Cero dependencias externas.
2. `src/Dentistry.Data` (Class Library): `DentistryDbContext`, configuraciones Fluent API, migraciones de PostgreSQL y servicios de infraestructura (PDF y Backups).
3. `src/Dentistry.Desktop` (WPF App): Vistas XAML, UserControls (Odontograma vectorial), ViewModels e inyección de dependencias. Usa `IDbContextFactory<DentistryDbContext>` para crear contextos de datos cortos por operación.
4. `tests/Dentistry.UnitTests` (xUnit): Pruebas unitarias enfocadas en `Dentistry.Core`.

## 4. Especificación Funcional (Basada en la Historia Clínica Física de "Dentistry")
El modelo de datos debe reflejar fielmente las 13 secciones del documento clínico oficial:

1. **Datos Generales (`Patient`):** Número de Historia, Fecha de diligenciamiento, Nombre completo, Cédula (índice único), Fecha de nacimiento (Edad calculada dinámicamente), Sexo (M/F), Estado civil, Ocupación, Dirección, Teléfono, Email, Persona responsable y Teléfono del responsable.
2. **Motivo de Consulta y Enfermedad Actual (`ClinicalHistory`):** Campos de texto para `ConsultationReason` y `CurrentIllness`.
3. **Antecedentes Personales o Alerta Médica (`MedicalAlerts` - JSONB en Postgres):** Booleans para Neurológicos, Cardiovasculares, Inmunológicos, Respiratorios, Gastrointestinal, Otorrinolaringológicos, Quirúrgicos, Hospitalarios, Farmacológicos + campo `Observations`. Si alguno es `true`, activar indicador visual de alerta médica en la UI.
4. **Examen Extraoral (`ExtraoralExam` - JSONB):** Booleans (SI/NO) para Masas, Adenopatías, Asimetría facial, ATM, Labios + `Observations`.
5. **Examen Intraoral (`IntraoralExam` - JSONB):** Booleans (Normal SI/NO) para Mucosas, Paladar, Frenillos, Glándulas salivales, Lengua, Carrillos, Piso de boca + `Observations`.
6. **Control Odontológico (`DentalControlInfo` - JSONB):** Consulta en el último año (SI/NO), Fecha, Motivo, Frecuencia de cepillado, Crema dental, Uso de hilo dental (SI/NO).
7. **Odontograma Interactivo (`OdontogramTooth`):** 
   - Compuesto por 52 piezas dentales (Nomenclatura FDI):
     - Adultos Superiores: 18..11 y 21..28 | Adultos Inferiores: 48..41 y 31..38.
     - Niños Superiores: 55..51 y 61..65 | Niños Inferiores: 85..81 y 71..75.
   - En WPF se implementará un `UserControl` vectorial reutilizable (`ToothControl.xaml`) con forma circular dividida en 5 zonas clicables (4 trapecios periféricos y 1 círculo central) más un recuadro de texto superior/inferior para anotaciones.
8. **Análisis Radiográfico, Diagnósticos y Pronóstico:** Tres campos de texto enriquecido/multilínea dentro de `ClinicalHistory` (Secciones 9, 10 y 11).
9. **Plan de Tratamiento (`TreatmentPlanItem` - Sección 12):** Lista de ítems asociados a la historia con `Procedure` (Procedimiento) y `Value` (`decimal` Valor).
10. **Aceptación de Tratamiento (Sección 13):** Datos del odontólogo tratante y generación de documento PDF con el párrafo legal ("Yo, [Nombre] identificado con C.C N° [Cédula]...") y espacios de firma para Paciente y Odontólogo.
11. **Control de Evolución, Abonos y Saldos (`PaymentLedgerEntry` - Tabla final):** Registro cronológico con `Date` (Fecha), `ProcedurePerformed` (Procedimiento), `PaymentAmount` (Abono en `decimal`) y `Balance` (Saldo restante calculado automáticamente restando los abonos al total del Plan de Tratamiento).

## 5. Flujo de Trabajo Paso a Paso
Trabajaremos de forma iterativa. No generes todo el sistema de golpe. Espera siempre mi confirmación antes de pasar a la siguiente fase:
- Fase 1: Estructura de proyectos, entidades en `Dentistry.Core` y pruebas unitarias de saldos con `xUnit`.
- Fase 2: Configuración de PostgreSQL, `DentistryDbContext` con columnas JSONB y primera migración en `Dentistry.Data`.
- Fase 3: Configuración de `HostBuilder` e Inyección de Dependencias en `Dentistry.Desktop` + CRUD de Pacientes.
- Fase 4: Formulario de Historia Clínica, Presupuesto y tabla de Abonos/Saldos.
- Fase 5: Creación del `ToothControl` vectorial para el Odontograma de 52 piezas y exportación a PDF.