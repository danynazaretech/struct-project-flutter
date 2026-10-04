# Publicando um aplicativo Flutter na Google Play

> **Guia prático para entender onde cada configuração fica — do projeto Flutter ao lançamento na loja.**

---

## Sumário

1. [Visão geral: as quatro camadas](#1-visão-geral-as-quatro-camadas)
2. [Estrutura básica do projeto](#2-estrutura-básica-do-projeto)
3. [Configurações do Flutter](#3-configurações-do-flutter)
4. [Nome, descrição e versão](#4-nome-descrição-e-versão)
5. [Configurações do Android](#5-configurações-do-android)
6. [Assets, logo e ícone](#6-assets-logo-e-ícone)
7. [Permissões e APIs](#7-permissões-e-apis)
8. [Build e release](#8-build-e-release)
9. [Configurações da Google Play](#9-configurações-da-google-play)
10. [Exemplo completo: Cozinha Fácil](#10-exemplo-completo-cozinha-fácil)
11. [O que fica onde?](#11-o-que-fica-onde)
12. [Checklist antes da publicação](#12-checklist-antes-da-publicação)
13. [Objetivo final](#13-objetivo-final)

---

## 1. Visão geral: as quatro camadas

Antes de preparar o aplicativo para publicação, é importante entender que **nem todas as informações ficam no mesmo arquivo**.

O projeto possui quatro camadas principais:

```text
┌─────────────────────────────────────────┐
│ 1. FLUTTER                               │
│                                         │
│ pubspec.yaml                            │
│ • name                                  │
│ • description                           │
│ • version                               │
│ • dependencies                          │
│ • assets                                │
└───────────────────┬─────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│ 2. ANDROID                              │
│                                         │
│ • applicationId                         │
│ • AndroidManifest.xml                   │
│ • permissões                            │
│ • ícone                                 │
│ • Gradle                                │
└───────────────────┬─────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│ 3. BUILD / RELEASE                      │
│                                         │
│ • APK                                   │
│ • AAB                                   │
│ • assinatura                            │
│ • keystore                              │
└───────────────────┬─────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│ 4. GOOGLE PLAY                          │
│                                         │
│ • nome comercial                        │
│ • descrição                             │
│ • screenshots                           │
│ • categoria                             │
│ • classificação                         │
│ • AAB                                   │
└─────────────────────────────────────────┘
```

### Em uma frase

> **Flutter define o projeto, o Android configura a plataforma, o release gera o pacote e a Google Play apresenta o produto ao usuário.**

---

## 2. Estrutura básica do projeto

Um projeto Flutter costuma ter uma estrutura semelhante a esta:

```text
meu_app/
│
├── android/
├── assets/
├── lib/
│   ├── main.dart
│   ├── screens/
│   ├── widgets/
│   ├── models/
│   └── services/
├── test/
├── pubspec.yaml
├── analysis_options.yaml
└── README.md
```

| Diretório ou arquivo | Responsabilidade |
|---|---|
| `lib/` | Código Dart e Flutter |
| `android/` | Configurações específicas do Android |
| `assets/` | Imagens, fontes e outros recursos |
| `test/` | Testes automatizados |
| `pubspec.yaml` | Configuração principal do projeto Flutter |
| `README.md` | Documentação do projeto |

---

## 3. Configurações do Flutter

### 3.1 O arquivo `pubspec.yaml`

O arquivo `pubspec.yaml` fica na raiz do projeto e concentra as principais configurações do Flutter, como:

- nome técnico do projeto;
- descrição técnica;
- versão;
- dependências;
- assets;
- configurações relacionadas ao Flutter.

### Exemplo

```yaml
name: cozinha_facil

description: Aplicativo desenvolvido para auxiliar usuários no planejamento e preparo de receitas.

publish_to: 'none'

version: 1.0.0+1

dependencies:
  flutter:
    sdk: flutter

flutter:
  uses-material-design: true

  assets:
    - assets/images/
    - assets/icon/
```

> **Atenção:** `pubspec.yaml` configura o projeto Flutter. Ele não substitui as configurações específicas do Android nem as informações comerciais da Google Play.

---

## 4. Nome, descrição e versão

### 4.1 Nome técnico do projeto

```yaml
name: cozinha_facil
```

Esse campo representa o **nome técnico do pacote/projeto Flutter**. Ele deve seguir as regras de nomenclatura dos pacotes Dart/Flutter.

Normalmente, usamos:

```yaml
name: cozinha_facil
```

em vez de:

```yaml
name: Cozinha Fácil
```

#### Não confunda

| Informação | Exemplo | Onde aparece |
|---|---|---|
| Nome técnico Flutter | `cozinha_facil` | `pubspec.yaml` |
| Nome exibido ao usuário | `Cozinha Fácil` | Configuração Android |
| Nome comercial da loja | `Cozinha Fácil` | Google Play Console |

---

### 4.2 Descrição técnica

No `pubspec.yaml`, podemos definir uma descrição associada ao projeto:

```yaml
description: Aplicativo desenvolvido para auxiliar usuários no planejamento e preparo de receitas.
```

Essa **não é automaticamente a descrição da Google Play Store**.

| Tipo de descrição | Onde é configurada |
|---|---|
| Descrição técnica do projeto | `pubspec.yaml` |
| Descrição curta da loja | Google Play Console |
| Descrição completa da loja | Google Play Console |

---

### 4.3 Versionamento

A versão pode ser definida assim:

```yaml
version: 1.0.0+1
```

A estrutura é:

```text
1.0.0+1
│ │ │ │
│ │ │ └── Build number
│ │ └──── Patch
│ └────── Minor
└──────── Major
```

Nesse exemplo:

- **Versão:** `1.0.0`
- **Build number:** `1`

#### Exemplos de atualização

```yaml
version: 1.0.1+2   # Correção ou pequeno ajuste
version: 1.1.0+3   # Nova funcionalidade compatível
version: 2.0.0+4   # Mudança maior
```

> **Regra importante:** a versão identifica o lançamento; o build number diferencia uma compilação da outra. Ao enviar uma nova versão para a loja, o build number deve ser incrementado.

---

## 5. Configurações do Android

### 5.1 Application ID

O `Application ID` é diferente do `name` do `pubspec.yaml`. Ele pertence à configuração Android e identifica tecnicamente o aplicativo.

Dependendo da estrutura do projeto, procure em:

```text
android/app/build.gradle
```

ou:

```text
android/app/build.gradle.kts
```

#### Groovy — `build.gradle`

```gradle
defaultConfig {
    applicationId "br.edu.ifsuldeminas.cozinhafacil"
}
```

#### Kotlin DSL — `build.gradle.kts`

```kotlin
defaultConfig {
    applicationId = "br.edu.ifsuldeminas.cozinhafacil"
}
```

Um exemplo de identificador é:

```text
br.edu.ifsuldeminas.cozinhafacil
```

### 5.2 Nome exibido no Android

O nome que aparece para o usuário no dispositivo pode ser configurado em:

```text
android/app/src/main/res/values/strings.xml
```

```xml
<resources>
    <string name="app_name">Cozinha Fácil</string>
</resources>
```

O `AndroidManifest.xml` pode utilizar esse recurso:

```xml
android:label="@string/app_name"
```

### 5.3 Application ID × nome do aplicativo

```text
Nome comercial:       Cozinha Fácil
Nome técnico Flutter: cozinha_facil
Application ID:       br.edu.ifsuldeminas.cozinhafacil
Versão:               1.0.0
Build:                1
```

Esses valores têm funções diferentes:

- **Cozinha Fácil** é o nome apresentado ao usuário;
- **`cozinha_facil`** é o nome técnico do projeto Flutter;
- **`br.edu.ifsuldeminas.cozinhafacil`** é o identificador técnico Android;
- **`1.0.0+1`** identifica a versão e o build do lançamento.

---

## 6. Assets, logo e ícone

### 6.1 Organização dos assets

As imagens e outros recursos podem ser organizados assim:

```text
assets/
├── images/
│   ├── imagem1.png
│   ├── imagem2.jpg
│   └── imagem3.png
└── icon/
    └── app_icon.png
```

Essas pastas precisam ser declaradas no `pubspec.yaml`:

```yaml
flutter:
  uses-material-design: true

  assets:
    - assets/images/
    - assets/icon/
```

Depois, execute:

```bash
flutter pub get
```

### 6.2 Imagem, logo e ícone

Esses elementos não são necessariamente a mesma coisa:

| Elemento | Finalidade |
|---|---|
| **Imagem** | Recurso utilizado dentro de uma tela |
| **Logo** | Elemento da identidade visual do aplicativo |
| **Ícone** | Recurso usado para identificar o aplicativo no Android |

Exemplo de uso de uma imagem no Flutter:

```dart
Image.asset('assets/images/logo.png')
```

Os recursos Android relacionados ao ícone ficam em:

```text
android/app/src/main/res/
```

### 6.3 Gerando o ícone do aplicativo

Uma alternativa é utilizar o pacote `flutter_launcher_icons`.

Exemplo de configuração:

```yaml
dev_dependencies:
  flutter_launcher_icons: ^[VERSÃO_ATUAL]

flutter_launcher_icons:
  android: true
  ios: false
  image_path: "assets/icon/app_icon.png"
```

Depois:

```bash
flutter pub get
dart run flutter_launcher_icons
```

> **Observação:** confira a versão atual do pacote no momento da instalação.

---

## 7. Permissões e APIs

### 7.1 `AndroidManifest.xml`

O arquivo:

```text
android/app/src/main/AndroidManifest.xml
```

é específico da plataforma Android. Nele podem ser declarados:

- permissões;
- componentes;
- configurações da aplicação;
- informações utilizadas pelo sistema Android.

Exemplo de permissão de acesso à Internet:

```xml
<uses-permission
    android:name="android.permission.INTERNET" />
```

### 7.2 Use apenas as permissões necessárias

As permissões devem estar relacionadas às funcionalidades reais do aplicativo:

```text
Funcionalidade
      ↓
Necessidade
      ↓
Permissão
```

Por exemplo:

```text
Aplicativo acessa uma API
          ↓
Necessita comunicação de rede
          ↓
Configuração/permissão correspondente
```

> **Pergunta que o aluno deve saber responder:** “Por que meu aplicativo precisa dessa permissão?”

Não adicione permissões sem necessidade.

### 7.3 Comunicação com uma API

Quando o aplicativo utiliza uma API externa, o fluxo é semelhante a este:

```text
┌───────────────┐
│ Flutter       │
└───────┬───────┘
        │ HTTPS
        ↓
┌───────────────┐
│ API / Backend │
└───────┬───────┘
        │
        ↓
     Resposta
        │
        ↓
┌───────────────┐
│ Flutter       │
└───────────────┘
```

### 7.4 Cuidado com `localhost`

Durante o desenvolvimento, a API pode estar em:

```text
http://localhost:3000/api
```

No celular, porém, `localhost` representa **o próprio dispositivo**, e não o computador do desenvolvedor:

```text
localhost ≠ computador do desenvolvedor
```

Em produção, a API deve estar disponível em um endereço acessível pelo dispositivo, normalmente usando HTTPS:

```text
https://api.exemplo.com
```

### 7.5 Prefira HTTPS

Quando houver comunicação com servidores, utilize comunicação segura sempre que aplicável:

```text
https://
```

em vez de:

```text
http://
```

Isso é especialmente importante ao transmitir:

- dados pessoais;
- credenciais;
- senhas;
- tokens;
- dados de formulários.

---

## 8. Build e release

A camada de build transforma o projeto em um pacote instalável e publicável.

### APK

Usado principalmente para instalação e testes diretos em dispositivos Android.

### AAB

O **Android App Bundle (`.aab`)** é o formato normalmente utilizado para envio à Google Play.

O processo envolve:

```text
Código Flutter
      ↓
Compilação
      ↓
Empacotamento
      ↓
Assinatura
      ↓
AAB
```

### APK × AAB

```text
APK
 ↓
Testes no Android

AAB
 ↓
Google Play Console
```

> **Importante:** a publicação exige uma configuração adequada de assinatura e, normalmente, o uso de um keystore. Proteja esses arquivos e suas credenciais.

---

## 9. Configurações da Google Play

As informações comerciais da loja são configuradas no **Google Play Console**, e não no `pubspec.yaml`.

Entre elas estão:

- nome comercial;
- descrição curta;
- descrição completa;
- ícone da loja;
- screenshots;
- categoria;
- classificação do conteúdo;
- informações do aplicativo;
- pacote AAB.

### Fluxo de publicação

```text
Projeto Flutter
      ↓
Configurações Flutter
      ↓
Configurações Android
      ↓
Build de release
      ↓
Assinatura
      ↓
AAB
      ↓
Google Play Console
      ↓
Google Play
```

---

## 10. Exemplo completo: Cozinha Fácil

Imagine que o grupo desenvolveu o aplicativo **Cozinha Fácil**.

### 10.1 `pubspec.yaml`

```yaml
name: cozinha_facil

description: Aplicativo móvel desenvolvido para auxiliar usuários no planejamento e preparo de receitas.

publish_to: 'none'

version: 1.0.0+1

environment:
  sdk: '>=3.0.0 <4.0.0'

dependencies:
  flutter:
    sdk: flutter

flutter:
  uses-material-design: true

  assets:
    - assets/images/
    - assets/icon/
```

### 10.2 Nome visual do Android

Arquivo:

```text
android/app/src/main/res/values/strings.xml
```

Conteúdo:

```xml
<resources>
    <string name="app_name">Cozinha Fácil</string>
</resources>
```

### 10.3 Application ID

Arquivo:

```text
android/app/build.gradle
```

ou:

```text
android/app/build.gradle.kts
```

Exemplo:

```gradle
defaultConfig {
    applicationId "br.edu.ifsuldeminas.cozinhafacil"
}
```

### 10.4 Ícone

Arquivo de origem:

```text
assets/icon/app_icon.png
```

Esse recurso pode ser utilizado para gerar os ícones Android.

### 10.5 Manifest

Arquivo:

```text
android/app/src/main/AndroidManifest.xml
```

Nesse arquivo, devem ser verificadas as configurações Android e as permissões necessárias.

### 10.6 Google Play Console

As informações comerciais podem ser preenchidas posteriormente no Google Play Console:

```text
Nome:
Cozinha Fácil

Descrição curta:
Organize suas receitas de forma simples.

Descrição completa:
[descrição comercial completa]

Ícone:
[ícone]

Screenshots:
[screenshot 1]
[screenshot 2]
[screenshot 3]
```

---

## 11. O que fica onde?

| O que quero configurar? | Onde configurar? |
|---|---|
| Nome técnico do projeto | `pubspec.yaml` |
| Descrição técnica | `pubspec.yaml` |
| Versão e build number | `pubspec.yaml` |
| Application ID | `android/app/build.gradle` ou `build.gradle.kts` |
| Permissões | `android/app/src/main/AndroidManifest.xml` |
| Assets | Diretório `assets/`, declarado no `pubspec.yaml` |
| Nome visual do Android | `android/app/src/main/res/values/strings.xml` ou configuração Android equivalente |
| Ícone Android | `android/app/src/main/res/` ou ferramenta de geração de ícones |
| Descrição comercial da loja | Google Play Console |
| Screenshots | Google Play Console |
| Categoria | Google Play Console |
| Pacote para publicação | AAB enviado ao Google Play Console |

### Mapa rápido

```text
name
 ↓
Flutter
 ↓
pubspec.yaml
```

```text
applicationId
 ↓
Android
 ↓
build.gradle / build.gradle.kts
```

```text
permissão
 ↓
Android
 ↓
AndroidManifest.xml
```

```text
versão
 ↓
Flutter / Android Release
 ↓
pubspec.yaml
```

```text
descrição comercial
 ↓
Google Play
 ↓
Play Console
```

---

## 12. Checklist antes da publicação

Antes de gerar o AAB, confirme:

- [ ] Nome técnico configurado
- [ ] Descrição técnica configurada
- [ ] Versão configurada
- [ ] Build number configurado
- [ ] Application ID configurado
- [ ] Nome visual do aplicativo configurado
- [ ] Assets declarados no `pubspec.yaml`
- [ ] Ícone configurado
- [ ] `AndroidManifest.xml` revisado
- [ ] Permissões revisadas e justificadas
- [ ] API configurada para produção
- [ ] `localhost` removido das configurações de produção
- [ ] HTTPS verificado
- [ ] Aplicação testada em um dispositivo Android
- [ ] Assinatura de release configurada

### Comandos de validação e build

```bash
flutter clean
flutter pub get
flutter analyze
flutter build apk --release
flutter build appbundle --release
```

### Resultado esperado

```text
APK
 ↓
Testes no Android

AAB
 ↓
Google Play Console
```

---

## 13. Objetivo final

Ao concluir o exercício, o aluno deverá saber responder:

| Pergunta | Resposta |
|---|---|
| Onde está o código? | `lib/` |
| Onde estão as configurações Flutter? | `pubspec.yaml` |
| Onde estão as configurações Android? | `android/` |
| Onde está o Application ID? | Gradle |
| Onde estão as permissões? | `AndroidManifest.xml` |
| Onde está a versão? | `pubspec.yaml` |
| Onde está a descrição comercial? | Google Play Console |
| Qual é o pacote para publicação? | AAB |

### Resultado esperado

```text
PROJETO FLUTTER
       ↓
CONFIGURAÇÃO
       ↓
ANDROID
       ↓
RELEASE
       ↓
ASSINATURA
       ↓
AAB
       ↓
GOOGLE PLAY CONSOLE
       ↓
GOOGLE PLAY
```

> **O objetivo não é apenas aprender a publicar um aplicativo. É compreender como uma aplicação desenvolvida em Flutter se transforma em um produto Android distribuível.**

---

## Regra de ouro

> **Nem tudo que pertence ao aplicativo fica no `pubspec.yaml`.**
>
> Sempre pergunte: **o que estou configurando e quem utiliza essa informação?**
>
> - Flutter?
> - Android?
> - Build?
> - Google Play?
