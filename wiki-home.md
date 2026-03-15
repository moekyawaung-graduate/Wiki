# Moe Kyaw Aung — Developer Wiki

> **This wiki** documents Moe Kyaw Aung's development philosophy, project conventions, workflows, and resources. Use it as a technical reference for all repositories in this GitHub organization.

---

## 📋 Table of Contents

- [About Me](#about-me)
- [Development Philosophy](#development-philosophy)
- [Android Project Standards](#android-project-standards)
- [Architecture Conventions](#architecture-conventions)
- [CI/CD Workflow](#cicd-workflow)
- [Web Projects](#web-projects)
- [Tools & Setup](#tools--setup)
- [Education & Background](#education--background)
- [Contact](#contact)

---

## About Me

**Moe Kyaw Aung** is a Senior Android Developer based in Tachileik, Myanmar, with 5+ years of professional experience delivering high-quality mobile applications.

| Field | Details |
|-------|---------|
| **Role** | Senior Android Developer |
| **Experience** | 5+ years |
| **Location** | Tachileik, Myanmar 🇲🇲 |
| **Primary Languages** | Kotlin, Java |
| **Portfolio** | [moekyawaung-graduate.github.io](https://moekyawaung-graduate.github.io) |
| **Gravatar** | [gravatar.com/moekyawaung2026](https://gravatar.com/moekyawaung2026) |

---

## Development Philosophy

### Core Principles

1. **Clean over clever** — Code is read far more than it is written. Readability and intent always win.
2. **Architecture first** — Investing in a clear architecture upfront saves multiples in maintenance.
3. **Test what matters** — Unit-test business logic rigorously; integration-test critical paths.
4. **Ship, iterate, improve** — Working software over perfect software. Perfectionism is debt.
5. **Automate the boring** — CI/CD, linting, formatting, and deployments should be fully automated.

### Code Quality Standards

- All Kotlin code follows [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html)
- Ktlint enforced on every commit via pre-commit hook
- No unresolved TODOs in `main` / `master` branch
- PR reviews required for all feature branches

---

## Android Project Standards

### Module Structure

```
app/
├── data/
│   ├── local/          # Room database, DAOs
│   ├── remote/         # Retrofit APIs, DTOs
│   └── repository/     # Repository implementations
├── domain/
│   ├── model/          # Domain models (pure Kotlin)
│   ├── repository/     # Repository interfaces
│   └── usecase/        # Business logic use cases
├── presentation/
│   ├── ui/             # Composables / Fragments / Activities
│   ├── viewmodel/      # ViewModels
│   └── navigation/     # NavGraph
└── di/                 # Hilt modules
```

### Dependency Stack

| Category | Library |
|----------|---------|
| Language | Kotlin (latest stable) |
| UI | Jetpack Compose + Material 3 |
| DI | Hilt |
| Navigation | Compose Navigation |
| Network | Retrofit + OkHttp |
| Local DB | Room |
| Async | Kotlin Coroutines + Flow |
| Images | Coil |
| Backend | Firebase (Auth, Firestore, FCM) |
| Testing | JUnit4, MockK, Turbine |

### Naming Conventions

```kotlin
// Classes — PascalCase
class UserProfileViewModel

// Functions & variables — camelCase  
fun fetchUserProfile(userId: String): Flow<User>

// Constants — SCREAMING_SNAKE_CASE
const val MAX_RETRY_COUNT = 3

// Composables — PascalCase, named like components
@Composable
fun UserProfileScreen(viewModel: UserProfileViewModel)

// Resources — snake_case with prefix
// Layouts:  screen_user_profile.xml
// Strings:  user_profile_title
// Drawables: ic_user_avatar, bg_card_gradient
```

---

## Architecture Conventions

### MVVM with Clean Architecture

```
UI Layer (Compose)
    ↕ StateFlow / Events
ViewModel Layer
    ↕ Use Cases
Domain Layer (pure Kotlin)
    ↕ Repository interfaces
Data Layer (Room / Retrofit / Firebase)
```

### State Management Pattern

```kotlin
// UI State — sealed class or data class
data class ProfileUiState(
    val isLoading: Boolean = false,
    val user: User? = null,
    val error: String? = null
)

// ViewModel exposes StateFlow
val uiState: StateFlow<ProfileUiState>
    = _uiState.asStateFlow()

// One-shot events via Channel
val events: Flow<ProfileEvent>
```

### Error Handling

All network/DB operations return `Result<T>` or are wrapped in a `sealed class`:

```kotlin
sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Error(val exception: Exception) : Result<Nothing>()
    object Loading : Result<Nothing>()
}
```

---

## CI/CD Workflow

### GitHub Actions Pipeline

Every push to `main` and all Pull Requests trigger:

```yaml
jobs:
  1. lint         → ktlint, Android Lint
  2. unit-tests   → ./gradlew test
  3. build        → ./gradlew assembleRelease
  4. deploy       → (on tag) Upload to Play Store internal track
```

### Branch Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Production-ready code only |
| `develop` | Integration branch |
| `feature/*` | New features |
| `fix/*` | Bug fixes |
| `release/*` | Release preparation |

### Commit Message Format

```
type(scope): short description

feat(auth): add biometric login support
fix(chat): resolve message ordering bug  
refactor(viewmodel): extract use case for profile fetch
chore(deps): bump Kotlin to 2.0.0
docs(readme): update setup instructions
```

---

## Web Projects

### Project Conventions

All web projects in this org are **self-contained HTML files** unless specified. This means:
- No build tools required
- No external dependencies except CDN-linked libraries
- Fully functional offline (where applicable)

### Featured Web Projects

#### 🌐 Moekyaw's Translator
- **Tech:** HTML, CSS, JS, Gemini API, Web Speech API
- **Features:** Multi-language translation, microphone input, TTS playback
- **File:** Single `index.html`

#### 🎮 3D Magic Cube
- **Tech:** Three.js (r128), JavaScript
- **Features:** Interactive 3D Rubik's Cube, scramble, step-by-step solve
- **File:** Single `magic-cube.html`

#### 📚 Burmese Learning Portal
- **Tech:** HTML, Bootstrap, Animate.css, FlexSlider
- **Content:** Programming Basics, MVP Design, GitHub usage (Burmese language)
- **File:** Multi-section `index.html`

---

## Tools & Setup

### Development Environment

```
OS:            Windows / Linux
IDE:           Android Studio (latest stable)
JDK:           17 (Temurin)
Kotlin:        2.x (latest stable)
AGP:           latest stable
Git:           2.x
Node.js:       LTS (for web projects)
```

### Android Studio Plugins

- **Kotlin** (bundled)
- **Ktlint** — code style enforcement
- **GitToolBox** — enhanced Git integration
- **Rainbow Brackets** — readability
- **Markdown** — wiki/README preview

### Recommended VSCode Extensions (for web projects)

- Live Server
- Prettier
- Auto Rename Tag
- Color Highlight

---

## Education & Background

| Degree | Institution | Location |
|--------|------------|---------|
| B.Sc. Computer Science | University of Computer Studies | Pathein, Myanmar |
| B.A. English | Pathein Distance University | Pathein, Myanmar |

**Languages:**
- 🇲🇲 Burmese — Native
- 🇬🇧 English — Professional proficiency

---

## Contact

| Platform | Link |
|----------|------|
| 🌐 Portfolio | [moekyawaung-graduate.github.io](https://moekyawaung-graduate.github.io) |
| 👤 Gravatar | [gravatar.com/moekyawaung2026](https://gravatar.com/moekyawaung2026) |
| 💼 GitHub | [github.com/moekyawaung-graduate](https://github.com/moekyawaung-graduate) |

---

*Last updated: 2026 · Maintained by Moe Kyaw Aung*
