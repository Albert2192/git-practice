# Guía de Migración Angular 16 → 17 → 18 → 19

**Proyecto:** siare-front-usuarios (ejemplo)  
**Versión Actual:** Angular 16.0.0  
**Versión Objetivo:** Angular 19  
**Fecha:** Diciembre 2025

---

## 📋 Tabla de Contenidos

1. [Resumen Ejecutivo](#resumen-ejecutivo)
2. [Análisis del Proyecto Actual](#análisis-del-proyecto-actual)
3. [Estrategia de Migración](#estrategia-de-migración)
4. [Pre-requisitos y Preparación](#pre-requisitos-y-preparación)
5. [Migración Paso a Paso](#migración-paso-a-paso)
   - [Fase 1: Angular 16 → 17](#fase-1-angular-16--17)
   - [Fase 2: Angular 17 → 18](#fase-2-angular-17--18)
   - [Fase 3: Angular 18 → 19](#fase-3-angular-18--19)
6. [Cambios Importantes por Versión](#cambios-importantes-por-versión)
7. [Checklist de Validación](#checklist-de-validación)
8. [Troubleshooting](#troubleshooting)
9. [Recursos Adicionales](#recursos-adicionales)

---

## 📊 Resumen Ejecutivo

### ¿Por qué migrar versión por versión?

Angular **NO recomienda saltar versiones** en las actualizaciones. Cada versión incluye:
- Scripts de migración automática (schematics)
- Corrección de breaking changes
- Deprecación gradual de APIs
- Actualizaciones de dependencias compatibles

### Timeline Estimado

| Fase | Duración Estimada | Complejidad |
|------|-------------------|-------------|
| Preparación | 1-2 días | Media |
| Angular 16 → 17 |  4 - 6 días | Alta |
| Angular 17 → 18 | 3 - 5 días | Media |
| Angular 18 → 19 | 3 - 5 días| Media |
| Pruebas y Ajustes | 5 - 7 días | Alta |
| **TOTAL** | **16 - 25 días** | - |

---

## 🔍 Análisis del Proyecto Actual

### Versiones Actuales Detectadas

```json
{
  "@angular/core": "^16.0.0",
  "@angular/cli": "^16.0.0",
  "typescript": "~5.0.4",
  "rxjs": "~7.8.1",
  "zone.js": "~0.13.1",
  "primeng": "16.0.2",
  "@ng-bootstrap/ng-bootstrap": "^15.1.1",
  "nx": "16.7.3"
}
```

### Dependencias Críticas

El proyecto utiliza:
- **Nx Monorepo**: Versión 16.7.3 (requerirá actualización)
- **PrimeNG**: Versión 16.0.2 (UI components)
- **ng-bootstrap**: Versión 15.1.1
- **Chart.js**: Versión 3.3.2
- **Leaflet**: Para mapas
- **RxJS**: Para programación reactiva
- **Multiple servicios personalizados**: ngx-siare-common, siare-model, siare-services

### Características del Proyecto

- ✅ TypeScript 5.0.4
- ✅ Usa módulos NgModule (tradicional)
- ✅ Routing modular
- ✅ Guards de autenticación
- ✅ Interceptores HTTP
- ✅ Servicios compartidos
- ⚠️ Dependencias privadas (ngx-siare-common, siare-services, etc.)

---

## 🎯 Estrategia de Migración

### Principios Fundamentales

1. **Migración Incremental**: Una versión mayor a la vez
2. **Control de Versiones**: Commit después de cada migración exitosa
3. **Testing Continuo**: Probar después de cada actualización
4. **Backup Completo**: Copias de seguridad antes de iniciar
5. **Ambiente Aislado**: Trabajar en rama separada

### Workflow Recomendado

```
┌─────────────────────────────────────────────────────┐
│ 1. Crear rama: feature/angular-16-to-17             │
│ 2. Actualizar Angular CLI y Core                    │
│ 3. Ejecutar migraciones automáticas                 │
│ 4. Actualizar dependencias relacionadas             │ 
│ 5. Resolver conflictos manualmente                  │
│ 6. Ejecutar tests (unit + e2e)                      │ 
│ 7. Build de producción                              │
│ 8. Commit y merge                                   │
│ 9. Repetir para siguiente versión                   │
└─────────────────────────────────────────────────────┘
```

---

## ⚙️ Pre-requisitos y Preparación

### 1. Backup Completo


### 2. Control de Versiones

```powershell
# Crear rama para la migración
git checkout -b feature/angular-16-to-19-migration
git push -u origin feature/angular-16-to-19-migration

# Asegurar que estamos en un estado limpio
git status
```

### 3. Limpiar Dependencias

```powershell
# Limpiar caché de npm
npm cache clean --force

# Eliminar node_modules y reinstalar
Remove-Item -Recurse -Force node_modules
Remove-Item -Force package-lock.json
npm install
```

### 4. Verificar y Actualizar Node.js

**⚠️ CRÍTICO:** Angular CLI verifica la versión de Node.js antes de ejecutar migraciones. Una versión incompatible hará que `ng update` falle.

#### Compatibilidad Node.js por Versión de Angular

| Angular | Node.js Requerido | npm Recomendado |
|---------|------------------|------------------|
| 16 | 16.14+ o 18.10+ | 8.x+ |
| **17** | **18.13+ o 20.9+** | **9.x+** |
| **18** | **18.19+ o 20.11+** | **10.x+** |
| **19** | **18.19+ o 20.11+ o 22.x** | **10.x+** |

#### Verificar Versión Actual

```powershell
# Node.js
node --version

# npm
npm --version
```

#### Actualizar Node.js

**Opción 1: Con nvm (Recomendado)**

```powershell
# Listar versiones disponibles
nvm list available

# Instalar Node.js 20 LTS (recomendado para Angular 17+)
nvm install 20.11.0

# Usar la versión instalada
nvm use 20.11.0

# Verificar
node --version
npm --version
```

**Opción 2: Descarga directa**
- Descargar desde: https://nodejs.org/
- Instalar versión LTS 20.x

#### Verificar Angular CLI Global

```powershell
# Angular CLI Global
npm list -g @angular/cli

# Actualizar CLI global si es necesario
npm install -g @angular/cli@latest
```

---

## 🚀 Migración Paso a Paso

## Fase 1: Angular 16 → 17

### Paso 1.0: Verificar Node.js

**⚠️ ANTES DE EMPEZAR:** Angular 17 requiere Node.js 18.13+ o 20.9+

```powershell
# Verificar versión actual
node --version

# Si es menor a 18.13, actualizar con nvm
nvm install 20.11.0
nvm use 20.11.0

# Verificar nuevamente
node --version  # Debe mostrar v20.11.0 o superior
npm --version   # Debe mostrar v10.x o superior
```

### 📌 Novedades Principales de Angular 17

Angular 17 es una **versión mayor con cambios significativos**:

1. **🎨 Nueva Sintaxis de Control Flow**
   - Reemplaza `*ngIf`, `*ngFor`, `*ngSwitch` con sintaxis `@if`, `@for`, `@switch`
   - Mejor rendimiento y legibilidad

2. **⚡ Deferrable Views**
   - Carga diferida de componentes con `@defer`
   - Mejora el rendimiento inicial

3. **🔥 Signals Estables**
   - Sistema de reactividad mejorado
   - Alternativa a RxJS en algunos casos

4. **📦 Nuevo Application Builder (esbuild)**
   - Build hasta 80% más rápido
   - Mejor tree-shaking

5. **🎭 SSR y Hydration Mejorados**
   - Renderizado del lado del servidor optimizado

### Paso 1.1: Actualizar Angular CLI y Core

```powershell
# Actualizar Angular CLI globalmente (opcional pero recomendado)
npm install -g @angular/cli@17

# Navegar al directorio del proyecto
cd c:\Users\usuarioprueba\Desktop\siare-front-usuarios

# Ejecutar actualización a Angular 17
ng update @angular/cli@17 @angular/core@17
```

**⚠️ Importante:** El comando anterior ejecutará automáticamente:
- Actualización de package.json
- Migraciones de código (schematics)
- Actualización de archivos de configuración

### Paso 1.2: Actualizar Otros Paquetes Angular

```powershell
# Verificar qué otros paquetes necesitan actualización
ng update

# Actualizar paquetes adicionales de Angular
ng update @angular/animations@17 @angular/cdk@17 @angular/platform-browser@17 @angular/router@17 @angular/forms@17
```

### Paso 1.3: Actualizar TypeScript

Angular 17 requiere TypeScript 5.2+:

```powershell
npm install typescript@~5.2.0 --save-dev
```

### Paso 1.4: Actualizar PrimeNG

```powershell
# PrimeNG para Angular 17
npm install primeng@17 primeicons@latest --save
```

### Paso 1.5: Actualizar ng-bootstrap

```powershell
# ng-bootstrap compatible con Angular 17
npm install @ng-bootstrap/ng-bootstrap@16 --save
```

### Paso 1.6: Actualizar Nx (si se usa en el proyecto)

```powershell
# Actualizar Nx a versión 17
npx nx migrate 17
npm install
npx nx migrate --run-migrations
```

### Paso 1.7: Actualizar Dependencias Manuales

**⚠️ Importante:** Angular CLI actualiza automáticamente los paquetes `@angular/*`, pero debes actualizar manualmente las librerías de terceros.

#### Dependencias que Angular actualiza automáticamente:
- ✅ Todos los paquetes `@angular/*` (core, router, forms, etc.)
- ✅ TypeScript (si está en el rango compatible)
- ✅ Zone.js
- ✅ RxJS (si es necesario)

#### Dependencias que DEBES actualizar manualmente:

```powershell
# 1. PrimeNG y PrimeIcons (UI Components)
npm install primeng@17 primeicons@latest --save

# 2. ng-bootstrap (Bootstrap para Angular)
npm install @ng-bootstrap/ng-bootstrap@16 --save

# 3. Librerías Angular específicas de tu proyecto
npm install angularx-qrcode@17 --save
npm install ngx-color-picker@17 --save
npm install ngx-editor@17 --save

# 4. Chart.js (Actualizar a v4 es opcional)
npm install chart.js@^4.0.0 --save

# 5. Bootstrap (actualizar a última versión de v5)
npm install bootstrap@^5.3.0 --save

# 6. Otras dependencias recomendadas
npm install @popperjs/core@latest --save
npm install leaflet@latest --save
npm install jwt-decode@^4.0.0 --save

# 7. DevDependencies
npm install @types/leaflet@latest --save-dev
```

#### Dependencias que pueden quedarse como están (verificar compatibilidad):
- ⚠️ `@elastic/apm-rum-angular` - Verificar compatibilidad con Angular 17
- ⚠️ `@ngx-matomo/tracker` - Verificar compatibilidad
- ⚠️ `ngx-extended-pdf-viewer` - Ya está en v17.4.6 (OK)
- ⚠️ Librerías privadas: `ngx-siare-common`, `siare-model`, `siare-services` - Actualizar según tu equipo

#### Dependencias obsoletas a considerar reemplazar:
```powershell
# jwt-decode v3 → v4 (breaking changes)
npm install jwt-decode@^4.0.0 --save

# moment → date-fns (opcional, date-fns es más moderno)
# Ya tienes date-fns instalado, considera migrar de moment
```

### Paso 1.8: Ajustes Manuales para Angular 17

#### a) Revisar uso de `@Injectable()`

Angular 17 es más estricto con la inyección de dependencias:

```typescript
// ❌ Antes (puede causar problemas)
@Injectable()
export class MiServicio {}

// ✅ Después (especificar providedIn)
@Injectable({
  providedIn: 'root'  // o 'any', o un módulo específico
})
export class MiServicio {}
```

#### b) Actualizar imports de RxJS

Verificar que no se usen imports obsoletos:

```typescript
// ✅ Correcto
import { Observable, Subject } from 'rxjs';
import { map, filter, switchMap } from 'rxjs/operators';
```

#### c) Revisar RouterModule

```typescript
// Verificar configuración de routing
RouterModule.forRoot(routes, {
  // Angular 17 usa por defecto:
  // enableTracing: false,
  // bindToComponentInputs: true (nuevo en 17)
})
```

### Paso 1.9: Compilar y Probar

```powershell
# Limpiar y reinstalar
Remove-Item -Recurse -Force node_modules, .angular
npm install

# Compilar
ng build --configuration=production

# Ejecutar tests
npm test

# Levantar en desarrollo
ng serve
```

### Paso 1.10: Resolver Errores Comunes

#### Error: "Can't resolve 'zone.js'"
```powershell
npm install zone.js@~0.14.0 --save
```

#### Error: TypeScript compilation errors
```powershell
# Verificar versión de TypeScript
npm list typescript

# Debe ser 5.2.x para Angular 17
npm install typescript@~5.2.0 --save-dev --save-exact
```

#### Error: "Workspace extension with invalid name"
Revisar `angular.json` y asegurar que no hay configuraciones obsoletas.

### Paso 1.11: Commit

```powershell
git add .
git commit -m "feat: Migración a Angular 17 completada"
git push
```

---

## Fase 2: Angular 17 → 18

### Paso 2.0: Verificar Node.js

**⚠️ ANTES DE EMPEZAR:** Angular 18 requiere Node.js 18.19+ o 20.11+

```powershell
# Verificar versión actual
node --version

# Si es menor a 18.19, actualizar con nvm
nvm install 20.11.0
nvm use 20.11.0

# Verificar nuevamente
node --version  # Debe mostrar v20.11.0 o superior
npm --version   # Debe mostrar v10.x o superior
```

### 📌 Novedades Principales de Angular 18

1. **🎯 Zoneless Change Detection (Experimental)**
   - Detección de cambios sin Zone.js
   - Mejor rendimiento

2. **📝 Material 3 Components**
   - Nuevos componentes con Material Design 3

3. **🔄 Signals Mejorados**
   - Computed signals más eficientes
   - Effect() mejorado

4. **🛠️ Nuevas APIs**
   - `afterRender()` y `afterNextRender()`
   - Mejoras en Server-Side Rendering

5. **⚡ Performance**
   - Hydration mejorado
   - Lazy loading optimizado

### Paso 2.1: Actualizar a Angular 18

```powershell
# Actualizar CLI global (opcional)
npm install -g @angular/cli@18

# Actualizar proyecto
ng update @angular/cli@18 @angular/core@18
```

### Paso 2.2: Actualizar TypeScript

Angular 18 requiere TypeScript 5.4+:

```powershell
npm install typescript@~5.4.0 --save-dev
```

### Paso 2.3: Actualizar Dependencias

```powershell
# Verificar actualizaciones necesarias
ng update

# PrimeNG para Angular 18
npm install primeng@18 --save

# ng-bootstrap para Angular 18
npm install @ng-bootstrap/ng-bootstrap@17 --save

# Actualizar Nx si se usa
npx nx migrate 18
npm install
npx nx migrate --run-migrations
```

### Paso 2.4: Actualizar Dependencias Manuales Angular 18

```powershell
# Librerías Angular específicas
npm install angularx-qrcode@18 --save
npm install ngx-color-picker@18 --save
npm install ngx-editor@18 --save

# Verificar y actualizar otras dependencias si es necesario
npm install chart.js@latest --save
npm install bootstrap@latest --save
```

### Paso 2.5: Ajustes Manuales Angular 18

#### a) Revisar uso de `inject()`

Angular 18 promueve el uso de `inject()` en lugar de constructor injection:

```typescript
// ✅ Enfoque moderno (recomendado)
export class MiComponente {
  private miServicio = inject(MiServicio);
  private router = inject(Router);
}

// ⚠️ Enfoque tradicional (aún válido)
export class MiComponente {
  constructor(
    private miServicio: MiServicio,
    private router: Router
  ) {}
}
```

#### b) Guards con funciones

Los Guards funcionales son ahora el estándar:

```typescript
// ✅ Guard funcional (recomendado)
export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  return authService.isAuthenticated();
};

// routes
{
  path: 'admin',
  canActivate: [authGuard]
}
```

#### c) Verificar configuración de `provideRouter`

Para nuevas aplicaciones standalone:

```typescript
// main.ts
bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes),
    provideHttpClient(withInterceptors([authInterceptor]))
  ]
});
```

### Paso 2.6: Compilar y Probar

```powershell
# Limpiar
Remove-Item -Recurse -Force node_modules, .angular
npm install

# Compilar
ng build --configuration=production

# Tests
npm test

# Servidor desarrollo
ng serve
```

### Paso 2.7: Commit

```powershell
git add .
git commit -m "feat: Migración a Angular 18 completada"
git push
```

---

## Fase 3: Angular 18 → 19

### Paso 3.0: Verificar Node.js

**⚠️ ANTES DE EMPEZAR:** Angular 19 requiere Node.js 18.19+, 20.11+ o 22.x

```powershell
# Verificar versión actual
node --version

# Si es menor a 18.19, actualizar con nvm
nvm install 20.11.0
nvm use 20.11.0

# O instalar Node.js 22 (última LTS)
nvm install 22
nvm use 22

# Verificar nuevamente
node --version  # Debe mostrar v20.11.0+ o v22.x
npm --version   # Debe mostrar v10.x o superior
```

### 📌 Novedades Principales de Angular 19

1. **🎉 Standalone Components por Defecto**
   - Módulos NgModule ya no son necesarios en nuevos proyectos

2. **⚡ Incremental Hydration**
   - Hidratación progresiva para SSR

3. **🔥 Mejor Developer Experience**
   - Hot Module Replacement (HMR) mejorado
   - Diagnósticos de error más claros

4. **🎨 Signal-based Components**
   - Signals como primitiva principal para reactividad

5. **📦 Build Performance**
   - Optimizaciones adicionales en esbuild

### Paso 3.1: Actualizar a Angular 19

```powershell
# Actualizar CLI global (opcional)
npm install -g @angular/cli@19

# Actualizar proyecto
ng update @angular/cli@19 @angular/core@19
```

### Paso 3.2: Actualizar TypeScript

Angular 19 requiere TypeScript 5.5+:

```powershell
npm install typescript@~5.5.0 --save-dev
```

### Paso 3.3: Actualizar Dependencias

```powershell
# Verificar actualizaciones
ng update

# PrimeNG para Angular 19
npm install primeng@19 --save

# ng-bootstrap para Angular 19
npm install @ng-bootstrap/ng-bootstrap@18 --save

# RxJS (si es necesario)
npm install rxjs@~7.8.0 --save

# Zone.js
npm install zone.js@~0.15.0 --save

# Actualizar Nx
npx nx migrate 19
npm install
npx nx migrate --run-migrations
```

### Paso 3.4: Actualizar Dependencias Manuales Angular 19

```powershell
# Librerías Angular específicas
npm install angularx-qrcode@19 --save
npm install ngx-color-picker@19 --save
npm install ngx-editor@19 --save

# Verificar compatibilidad de otras librerías
npm outdated

# Actualizar las que sean compatibles
npm install chart.js@latest bootstrap@latest leaflet@latest --save
```

### Paso 3.5: Ajustes Manuales Angular 19

#### a) Considerar migración a Standalone

Angular 19 favorece componentes standalone:

```typescript
// ✅ Componente standalone
@Component({
  selector: 'app-usuarios-list',
  standalone: true,
  imports: [CommonModule, FormsModule, RouterModule],
  templateUrl: './usuarios-list.component.html'
})
export class UsuariosListComponent {}
```

#### b) Usar Signals para Estado

```typescript
// ✅ Usando signals para estado reactivo
export class MiComponente {
  count = signal(0);
  doubleCount = computed(() => this.count() * 2);
  
  increment() {
    this.count.update(value => value + 1);
  }
}
```

#### c) Configuración de Aplicación

Revisar `main.ts` para aprovechar nueva configuración:

```typescript
// main.ts con configuración moderna
import { bootstrapApplication } from '@angular/platform-browser';
import { provideRouter } from '@angular/router';
import { provideHttpClient, withInterceptors } from '@angular/common/http';

bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes),
    provideHttpClient(
      withInterceptors([authInterceptor])
    ),
    // otros providers
  ]
});
```

### Paso 3.6: Optimizaciones Opcionales

#### a) Activar características experimentales

```typescript
// angular.json
{
  "projects": {
    "app": {
      "architect": {
        "build": {
          "options": {
            "experimentalZoneless": true  // Si quieres trabajar sin Zone.js
          }
        }
      }
    }
  }
}
```

#### b) Revisar y optimizar imports

```typescript
// Usar imports específicos para tree-shaking
import { map } from 'rxjs/operators';  // ✅
// en lugar de
import * as operators from 'rxjs/operators';  // ❌
```

### Paso 3.7: Compilar y Probar

```powershell
# Limpiar todo
Remove-Item -Recurse -Force node_modules, .angular, dist
npm install

# Build de producción
ng build --configuration=production

# Verificar bundle size
# Los bundles deben ser similares o más pequeños

# Tests completos
npm test

# E2E tests si los tienes
npm run e2e

# Servidor de desarrollo
ng serve
```

### Paso 3.8: Auditoría Final

```powershell
# Verificar vulnerabilidades
npm audit

# Corregir si es necesario
npm audit fix

# Listar versiones finales
npm list @angular/core @angular/cli typescript
```

### Paso 3.9: Commit Final

```powershell
git add .
git commit -m "feat: Migración a Angular 19 completada - Versión estable"
git push

# Crear tag de versión
git tag -a v2.0.0-angular19 -m "Migración a Angular 19 completada"
git push origin v2.0.0-angular19
```

---

## 📊 Cambios Importantes por Versión

### Matriz de Compatibilidad

| Característica | Angular 16 | Angular 17 | Angular 18 | Angular 19 |
|----------------|------------|------------|------------|------------|
| TypeScript | 5.0-5.1 | 5.2-5.3 | 5.4 | 5.5+ |
| Node.js | 16.x-18.x | 18.x-20.x | 18.x-20.x | 18.x-20.x |
| RxJS | 7.5+ | 7.8+ | 7.8+ | 7.8+ |
| Zone.js | 0.13.x | 0.14.x | 0.14.x | 0.15.x |
| Signals | Experimental | Estable | Mejorado | Principal |
| Standalone | Opcional | Recomendado | Recomendado | Por defecto |
| Control Flow | `*ngIf/For` | `@if/@for` | `@if/@for` | `@if/@for` |

---

## ✅ Checklist de Validación

### Después de Cada Migración

#### Build y Compilación
- [ ] `ng build` completa sin errores
- [ ] `ng build --configuration=production` funciona
- [ ] No hay warnings críticos en consola
- [ ] Bundle size no aumentó drásticamente

#### Tests
- [ ] Tests unitarios pasan (`npm test`)
- [ ] Tests E2E pasan (si existen)
- [ ] No hay console.errors en tests

#### Funcionalidad
- [ ] Login funciona correctamente
- [ ] Navegación entre rutas funciona
- [ ] Guards de autenticación funcionan
- [ ] Interceptores HTTP funcionan
- [ ] Formularios reactivos funcionan
- [ ] Servicios cargan datos correctamente
- [ ] Componentes de UI (PrimeNG) funcionan

#### Performance
- [ ] Tiempo de carga inicial aceptable
- [ ] No hay memory leaks evidentes
- [ ] Detección de cambios funciona bien
- [ ] Lazy loading funciona

#### Desarrollo
- [ ] `ng serve` levanta sin problemas
- [ ] Hot reload funciona
- [ ] Source maps funcionan
- [ ] Debugging en VS Code funciona

---

## 🔧 Troubleshooting

### Errores Comunes y Soluciones

#### 1. Error: "Cannot find module '@angular/...'"

**Solución:**
```powershell
Remove-Item -Recurse -Force node_modules
Remove-Item -Force package-lock.json
npm cache clean --force
npm install
```

#### 2. Error: "This version of CLI is only compatible with Angular versions..."

**Solución:**
```powershell
# Desinstalar CLI global
npm uninstall -g @angular/cli

# Instalar versión correcta
npm install -g @angular/cli@17  # o la versión que necesites
```

#### 3. Error: TypeScript version mismatch

**Solución:**
```powershell
# Instalar versión exacta requerida
npm install typescript@~5.2.0 --save-dev --save-exact

# Verificar
npm list typescript
```

#### 4. Error: "NgModule not found"

**Causa:** Imports incorrectos después de migración a standalone

**Solución:**
```typescript
// Asegurar imports correctos
import { NgModule } from '@angular/core';

// O migrar a standalone
@Component({
  standalone: true,
  imports: [CommonModule, FormsModule]
})
```

#### 5. Error: PrimeNG components no funcionan

**Solución:**
```powershell
# Asegurar versión compatible
npm install primeng@17 primeicons@latest

# Verificar imports en componente
import { ButtonModule } from 'primeng/button';
```

#### 6. Error: "Nx migration failed"

**Solución:**
```powershell
# Revertir migración
git checkout -- nx.json package.json

# Intentar manualmente
npm install @nx/workspace@17 --save-dev
```

---

## 📚 Recursos Adicionales

### Documentación Oficial

1. **Angular Update Guide**
   - URL: https://angular.dev/update-guide
   - Herramienta interactiva con instrucciones específicas


### Herramientas Recomendadas

1. **Angular CLI**
   ```powershell
   ng update  # Ver actualizaciones disponibles
   ng version # Ver versiones instaladas
   ```

2. **npm-check-updates**
   ```powershell
   npm install -g npm-check-updates
   ncu  # Ver actualizaciones disponibles
   ```


### Compatibilidad de Librerías

Verificar compatibilidad de librerías de terceros:

| Librería | Angular 16 | Angular 17 | Angular 18 | Angular 19 |
|----------|------------|------------|------------|------------|
| PrimeNG | 16.x | 17.x | 18.x | 19.x |
| ng-bootstrap | 15.x | 16.x | 17.x | 18.x |
| Angular Material | 16.x | 17.x | 18.x | 19.x |
| ngx-charts | 20.x | 20.x | 20.x | 20.x |
| Chart.js | 3.x | 3.x/4.x | 4.x | 4.x |

---


## 📋 Resumen de Comandos

### Comandos Esenciales por Fase

#### Preparación
```powershell
# Crear rama
git checkout -b feature/angular-migration

# Limpiar
Remove-Item -Recurse -Force node_modules
npm cache clean --force
npm install
```

#### Migración Angular 16 → 17
```powershell
ng update @angular/cli@17 @angular/core@17
npm install typescript@~5.2.0 --save-dev
npm install primeng@17 --save
ng build --configuration=production
npm test
git commit -m "feat: Migración a Angular 17"
```

#### Migración Angular 17 → 18
```powershell
ng update @angular/cli@18 @angular/core@18
npm install typescript@~5.4.0 --save-dev
npm install primeng@18 --save
ng build --configuration=production
npm test
git commit -m "feat: Migración a Angular 18"
```

#### Migración Angular 18 → 19
```powershell
ng update @angular/cli@19 @angular/core@19
npm install typescript@~5.5.0 --save-dev
npm install primeng@19 --save
ng build --configuration=production
npm test
git commit -m "feat: Migración a Angular 19"
```

#### Verificación Final
```powershell
npm audit
ng version
npm list --depth=0
git tag -a v2.0.0-angular19 -m "Angular 19 Migration Complete"
```

---

## 📝 Notas Finales

### Consideraciones Importantes

1. **Tiempo**: La migración puede tomar entre 16-25 días laborales
2. **Riesgos**: Principalmente en librerías de terceros no actualizadas
3. **Testing**: Crítico probar exhaustivamente después de cada fase
4. **Rollback**: Mantener plan B por si algo falla
5. **Monitoreo**: Vigilar aplicación después del despliegue

**Fecha:** Diciembre 2025  
**Versión del documento:** 1.0

---

## Buena suerte con la migración!

 
