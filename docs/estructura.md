# Estructura del proyecto

Base independiente del lenguaje y framework. Las carpetas se conservan en Git con `.gitkeep`; se pueden retirar al agregar archivos reales.

```text
apps/
  web/
    public/                    Recursos estáticos
    src/
      components/              Componentes reutilizables
      layouts/                 Estructuras de navegación
      services/                Cliente de la API
      styles/                  Estilos generales
      features/
        dashboard/             Saldos, vencimientos y exposición en MXN
        pedimentos/            Registro de entradas
        inventario/            Consulta de materiales y movimientos
        produccion/            BOM, consumos, merma y reciclado
        exportaciones/         Registro de salidas
        alertas/               Avisos a 90, 60 y 30 días
        cierre-mensual/        Excepciones, revisión y aprobación
        reportes/              Consulta y descarga de reportes
        auditoria/             Historial y vista de solo lectura
  api/
    src/
      modules/
        empresas/              Razones sociales, RFC y aislamiento de datos
        usuarios/              Acceso y roles
        pedimentos/            Entradas y partidas importadas
        ledger/                Registro único de movimientos y saldos
        produccion/            BOM, consumos, merma y reciclado
        exportaciones/         Salidas y vínculo con materiales importados
        peps/                  Asignación de consumos por antigüedad
        vencimientos/          Reglas configurables y fechas límite
        riesgo/                Estimación de exposición en MXN
        conciliacion/          Comparación de fuentes y excepciones
        cierre-mensual/        Estados del cierre y revisión del especialista
        reportes/              Generación de reportes
        auditoria/             Quién cambió qué, cuándo y por qué
      integrations/
        sat-data-stage/        Lectura y normalización de archivos
        agente-aduanal/        Lectura y normalización de archivos
        asistente-ia/          Explicación opcional de excepciones
      jobs/                    Tareas programadas para alertas y reportes
      shared/                  Configuración, errores y utilidades comunes
contracts/
  api/                         Contratos entre frontend y backend
  importaciones/               Esquemas de archivos de entrada
database/
  migrations/                  Cambios versionados del modelo de datos
  seeds/                       Datos iniciales reproducibles
data/
  samples/
    moldeo-inyeccion/           Escenario con merma y reciclado
    ensamble/                  Escenario de arneses o electrónicos
    multi-rfc/                 Escenario con varias razones sociales
  templates/                   Plantillas de carga de información
docs/
  arquitectura/                Modelo de datos, diagramas y decisiones
  negocio/                     Flujos, reglas, roles y criterios de aceptación
  demo/                        Guion, preparación y evidencias
  presentacion/                Diapositivas y recursos de exposición
tests/
  unit/                        PEPS, fechas, saldos y cálculo de riesgo
  integration/                 Importación, persistencia y conciliación
  e2e/                         Flujo completo y permisos por empresa
infra/                         Configuración futura de ejecución y despliegue
scripts/                       Utilidades de desarrollo y carga de demo
```

## Límites entre módulos

- `pedimentos`, `produccion` y `exportaciones` registran operaciones mediante `ledger`, que centraliza los movimientos.
- `peps` calcula asignaciones de consumo; una reversión debe conservar el historial.
- `vencimientos` resuelve fechas conforme a reglas configuradas; `riesgo` documenta los supuestos de la estimación monetaria.
- `conciliacion` compara las fuentes antes de incorporar los registros aprobados.
- `auditoria` registra los cambios de los módulos de negocio.
- Cada operación y consulta debe respetar la empresa/RFC y el rol del usuario.

La estructura reserva espacio para estas responsabilidades; no contiene implementaciones ni integraciones conectadas.
