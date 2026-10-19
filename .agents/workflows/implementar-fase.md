---
description: Ejecuta paso a paso una fase del desarrollo de DentistrySystem explicando cada cambio
---

# Flujo de Trabajo Iterativo — DentistrySystem

Cuando el usuario invoque este workflow indicando qué fase o módulo desea construir, ejecuta estrictamente esta secuencia:

1. **Análisis Previo y Explicación:**
   - Indica qué archivos existentes de la solución (`Dentistry.Core`, `Dentistry.Data`, `Dentistry.Desktop` o `Dentistry.UnitTests`) vas a modificar y por qué.

2. **Implementación Guiada por Capas:**
   - **Fase 1 (Dominio y Pruebas):** Implementar `Enums`, `ValueObjects` y `Entities` en `src/Dentistry.Core` y sus pruebas unitarias en `tests/Dentistry.UnitTests`.
   - **Fase 2 (Datos y PostgreSQL):** Implementar `DentistryDbContext`, configuraciones Fluent API (JSONB, índices únicos y `numeric(12,2)`) en `src/Dentistry.Data` y generar la migración.
   - **Fase 3 (Arranque WPF y Pacientes):** Configurar Inyección de Dependencias (`HostBuilder` + `IDbContextFactory`) en `App.xaml.cs` y el módulo CRUD de `PatientListView`.
   - **Fase 4 (Historia Clínica y Finanzas):** Implementar `ClinicalHistoryView` y `ClinicalHistoryViewModel` (Secciones 2 a 7 y 9 a 13, alertas médicas, presupuesto y abonos/saldos).
   - **Fase 5 (Odontograma y PDF):** Implementar el control vectorial `ToothControl.xaml`, la cuadrícula de 52 piezas en `OdontogramView.xaml` y el reporte PDF en `ClinicalHistoryPdfService.cs`.

3. **Verificación Técnica:**
   - Ejecuta `dotnet build` y `dotnet test` en la terminal para comprobar que hay 0 errores de compilación y que todas las pruebas unitarias pasan en verde.

4. **Resumen de Mentoría:**
   - Explica al usuario los conceptos clave aplicados en este paso y espera su confirmación antes de tocar el siguiente módulo.
