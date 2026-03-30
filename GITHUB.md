# GitHub — Cómo trabajamos

---

## Ramas permanentes

| Rama | Para qué sirve | Quién pushea aquí |
|---|---|---|
| `main` | Código que funciona al 100% | Nadie directamente — solo desde `dev` via PR |
| `dev` | Integración del equipo | Nadie directamente — solo desde ramas `feat/` via PR |

---

## 0. Clonar y configurar por primera vez

Cuando te unas al proyecto, el primer paso es bajar el código y prepararlo. Solo tienes que hacerlo una vez.

```bash
# 1. Clona el repositorio
git clone URL_DEL_REPOSITORIO
cd NOMBRE_DE_LA_CARPETA

# 2. Instala las dependencias
npm install

# 3. Configura las variables de entorno
cp .env.example .env
# IMPORTANTE: Abre .env y pide las claves reales a un compañero
```

---

## Antes de empezar cualquier día

Sincroniza siempre con `dev` antes de ponerte a trabajar.
Si no lo haces, tu rama se desactualiza y los merges se complican.

```bash
git checkout dev
git pull origin dev
git checkout tu-rama-actual
git merge dev
```

---

## Crear una rama nueva

Siempre desde `dev`, nunca desde `main`.

```bash
git checkout dev
git pull origin dev
git checkout -b feat/nombre-de-lo-que-haces
```

---

## Nombres de ramas

```bash
feat/salon-profile        # nueva funcionalidad
feat/booking-flow
feat/chat
fix/crash-al-reservar     # corrección de bug
chore/actualizar-deps     # mantenimiento, limpieza
```

Usa kebab-case (guiones, sin espacios, sin mayúsculas).

---

## Commits — cómo escribirlos

Un commit por cada cosa que funciona. No acumules todo al final del día.

```bash
git add .
git commit -m "feat: crear componente SalonCard"
git commit -m "feat: conectar búsqueda con salons.service"
git commit -m "fix: corregir crash al abrir chat vacío"
git commit -m "chore: regenerar tipos de Supabase"
```

**Prefijos:**
- `feat:` — algo nuevo que funciona
- `fix:` — corrección de un bug
- `chore:` — tareas que no son código de producto (deps, tipos, config)

---

## Subir tu rama y abrir un PR

```bash
# Subir tu rama
git push origin feat/nombre-de-tu-rama

# Luego en GitHub:
# 1. Ir a Pull Requests → New Pull Request
# 2. Base: dev  ←  Compare: feat/tu-rama
# 3. Título descriptivo: "Pantalla de perfil de salón con fotos"
# 4. Asignar a alguien del equipo para revisar
# 5. Crear PR
```

---

## Revisar un PR

Cuando alguien te asigna un PR para revisar:

1. Lee los cambios en la pestaña **Files changed**
2. Comprueba que no hay `import { supabase }` fuera de `src/services/`
3. Comprueba que no hay componentes duplicados que ya existen en `src/components/ui/`
4. Si todo bien → **Approve** → el autor mergea
5. Si hay algo raro → deja un comentario, no lo rechaces sin explicar

---

## Resolución de conflictos

Si al mergear `dev` en tu rama hay conflictos:

```bash
git checkout dev
git pull origin dev
git checkout feat/tu-rama
git merge dev
# Git marcará los conflictos en los ficheros afectados
# Ábrelos, busca los <<<< ==== >>>> y elige qué versión queda
git add .
git commit -m "chore: resolver conflictos con dev"
```

Si el conflicto es en un fichero compartido como `supabase.ts` o `icons.tsx`, avisa antes de resolverlo solo.

---

## Qué hacer si rompiste algo

```bash
# Ver el historial reciente
git log --oneline -10

# Volver al commit anterior sin perder los cambios
git reset --soft HEAD~1

# Volver al commit anterior descartando los cambios (cuidado)
git reset --hard HEAD~1
```

Si ya subiste la rama y necesitas deshacerlo, avisa al equipo antes de hacer force push.

---

## Reglas que no se rompen

- Nadie pushea directamente a `main` ni a `dev`
- Toda funcionalidad nueva va en su propia rama
- Antes de crear algo, comprueba que no existe ya en el proyecto
- Si tocas un fichero compartido (`supabase.ts`, `icons.tsx`, `constants.ts`), avisa al grupo
- Los PR van hacia `dev`, nunca directamente a `main`
