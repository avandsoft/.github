# 1. Avandsoft

**Avandsoft is a technology company and the umbrella organization for the AI, data-driven, research, and software-engineering projects.**

> **Avandsoft is building a research-driven AI ecosystem for decision-making, where reinforcement-learning research is transformed into an API through Colon and delivered to users through Semicolon as practical decision and investment support.**

Its general technical direction is:

1. **Artificial intelligence and machine learning**
2. **Automated decision support**
3. **Reinforcement learning**
4. **Intelligent automation**
5. **Software engineering and backend systems**
6. **Research-oriented AI systems**

The broader vision is not simply to build conventional software products. Avandsoft is also being used as a place to **research decision-making algorithms, implement those algorithms as software, and expose the resulting technology through practical products and APIs**.

The general pipeline is:

```text
Research
   ↓
AI / Reinforcement-Learning Algorithm
   ↓
Colon
   ↓
API
   ↓
Semicolon
   ↓
Practical Decision Support
```

---

## 2. The relationship between Avandsoft, Semicolon, and Colon

### Avandsoft

The company and umbrella for the research and products.

### Semicolon

The **user-facing decision-support platform**.

It is written primarily in **C# / ASP.NET Razor Pages**.

Semicolon is responsible for:

* users
* accounts
* decision scenarios
* factors
* choices
* questionnaires
* credits
* personalized reports
* investment-related functionality
* crypto monitoring
* commodity information
* stop-loss/take-profit monitoring
* email notifications
* presenting decision results to users

### Colon

Colon is **not the decision-scenario application itself**.

Colon is the **research-based decision engine exposed as an API**.

It is implemented with **Python/FastAPI** and contains the decision algorithm.

Semicolon structures the user's decision and sends the appropriate structured data to Colon.

Colon returns the algorithmic decision.

This distinction is important:

> **Semicolon understands the user's decision scenario. Colon calculates the decision from the structured input it receives.**

---

## 3. Colon

Colon is intentionally very small at the API surface.

It currently exposes **one endpoint**:

```text
/decision
```

The API receives a structured decision containing **exactly two choices**.

Conceptually:

```json
{
  "choices": [
    {
      "high_outcome": 0,
      "high_probability": 1,
      "low_outcome": 0,
      "distribution": 0,
      "quantity": 1
    },
    {
      "high_outcome": 0,
      "high_probability": 1,
      "low_outcome": 0,
      "distribution": 0,
      "quantity": 1
    }
  ]
}
```

The important constraint is:

> **Every Colon decision request contains exactly two choices.**

Colon returns something conceptually like:

```json
{
  "index": 1,
  "action": "first",
  "choice": {
    "high_outcome": 0,
    "high_probability": 1,
    "low_outcome": 0,
    "distribution": 0,
    "quantity": 1
  }
}
```

So Colon's responsibility is essentially:

```text
structured probabilistic choices
             ↓
          OptiBRL
             ↓
       selected action
```

It does not need to know the complete UI scenario, factor names, explanatory text, or the user's presentation layer.

---

## 4. OptiBRL

**OptiBRL is the reinforcement-learning algorithm used by Colon.**

This is the research core of the ecosystem.

Our research question is not simply:

> "Which choice has the highest predicted/expected value?"

We are investigating whether **Expected Value maximization is always the best decision strategy in uncertain and probabilistic environments**, particularly when decisions are made over time and under competitive/uncertain conditions.

Our research compares OptiBRL against baseline approaches and, according to our research results, demonstrates better performance in probabilistic environments over time.

The intended progression is:

```text
Decision theory / research
          ↓
      OptiBRL
          ↓
    Experimental study
          ↓
    Research paper
          ↓
       Colon
          ↓
     Semicolon
```

Our  larger objective is to make the algorithm useful beyond the research paper—to expose it through an API and eventually use it to provide decision support to real users.

---

## 5. Investment

This is the clarification we just made, and it is important.

**Investment is a feature/domain inside Semicolon.**

It includes market-data acquisition, processing, decision-making, monitoring, and user notification.

The architecture is:

```text
Semicolon — Investment
│
├── Market Data
│   ├── Cryptocurrency
│   │   ├── Binance API
│   │   └── Binance WebSocket
│   │
│   └── Commodities
│       └── Twelve Data
│
├── Market Data Processing
│
├── Structured Decision Input
│
├── Colon
│   └── OptiBRL
│
├── Investment Advice
│   └── Keep / Buy
│
└── Monitoring
    ├── Stop Loss
    └── Take Profit
```

### The critical point

Semicolon does **not** simply send raw market data to Colon.

Instead:

```text
Raw market data
       ↓
Semicolon extracts relevant information
       ↓
Structured decision data
       ↓
Colon
       ↓
OptiBRL
       ↓
Investment decision
```

So Semicolon owns the investment context and prepares the data.

Colon owns the decision algorithm.

---

## 6. Semicolon

Semicolon is the **main user-facing decision-support product under Avandsoft**.

Its fundamental purpose is:

> **Help users make better decisions using structured decision scenarios and AI/research-based decision technology.**

---

## 7. Decision scenarios

Decision scenarios are one of the core Semicolon concepts.

A scenario represents a decision.

A scenario contains:

```text
Scenario
 ├── Factors
 └── Choices
       ├── Choice 1
       └── Choice 2
```

The domain rule is strict:

> **A Scenario must contain exactly TWO Choices.**

Every choice must have a specification for **every factor used by the scenario**.

We previously defined the factor specification such that:

* `Worst` is a decimal score
* positive values represent beneficial impact
* negative values represent harmful impact
* zero represents no impact

This allows Semicolon to represent a human-readable decision in a structured mathematical form.

For example, conceptually:

```text
Scenario
    ↓
Factors
    ├── Cost
    ├── Time
    ├── Risk
    └── Flexibility

Choices
    ├── Option A
    └── Option B
```

Each choice gets a specification for every factor.

Semicolon can then transform that decision into the structured input required by Colon.

---

## 8. Semicolon → Colon decision flow

The actual architecture is approximately:

```text
User
 │
 ▼
Semicolon
 │
 │  scenario + factors + choices
 ▼
Decision representation
 │
 │  probabilistic representation
 ▼
Colon /decision
 │
 ▼
OptiBRL
 │
 ▼
Decision result
 │
 ▼
Semicolon
 │
 ▼
User
```

This means Semicolon is the **application and domain layer**, while Colon is the **decision computation engine**.

---

## 9. Questionnaires are separate

This was an important clarification we made.

**Questionnaires are not part of the decision algorithm.**

They should not be conceptually described as inputs to Colon or OptiBRL.

Their primary purposes are:

1. Allow users to answer questionnaires.
2. Generate a personalized report based on their answers.
3. Allow users to earn credits.

The questionnaire result can describe characteristics reflected by a user's answers and may be dynamically written using an LLM.

---

## 10. Personalized questionnaire reports

The questionnaire answers can be processed to produce a personalized textual report.

The report is generated dynamically using LLM technology.

So the flow is approximately:

```text
User
 ↓
Questionnaire
 ↓
Answers
 ↓
LLM processing
 ↓
Personalized report
```

The report is intended to illustrate characteristics reflected in the user's answers.

It is separate from the actual decision-support engine.

---

## 11. Credits

Semicolon has an internal **credit system**.

A credit is essentially:

> **Doxa is a unit consumed when a user requests an AI decision/advice service.**

Credits are deliberately **not represented as dollars or another real-world currency**. (A native cryptocurrency is planned for the future.)

A newly registered user receives some welcome credits.

Users can increase their credit balance by participating in eligible questionnaires.

Conceptually:

```text
Registration
    ↓
Welcome credits

Questionnaire
    ↓
Earn credits

Decision / AI advice
    ↓
Consume credits
```

Credits are therefore part of the Semicolon service economy, but they are not intended to represent real-world monetary.

---

## 12. LLM functionality

LLMs are used in Semicolon primarily for dynamic/personalized textual generation, particularly questionnaire-related reports.
This is separate from OptiBRL.
So there are two distinct AI technologies serving different purposes:

```text
LLM
 ↓
Natural-language / personalized report


OptiBRL
 ↓
Algorithmic decision-making
```

I would therefore not describe Semicolon as "an LLM decision-making platform." Its core decision engine is based on our research algorithm through Colon.

## 13. Investment decision support

Semicolon also contains an investment-related decision-support component.

This is separate from the general daily decision-scenario functionality, although both ultimately belong to the broader decision-support philosophy.

The investment functionality concerns assets such as:

* cryptocurrencies
* commodities

The system can provide information and decision support around investment horizons.

We have described functionality around decisions such as whether a user should:

* keep
* buy

supported assets.

The important distinction is that Semicolon is **not merely a price-prediction website**.

The broader research philosophy is:

```text
Commodity market data
        ↓
Semicolon
        ↓
Extract relevant information
        ↓
Structured input
        ↓
Colon
        ↓
OptiBRL
        ↓
Investment advice
```

---

## 14. Cryptocurrency & Commodity data

Semicolon maintains cryptocurrency-related market information.

We use **Binance APIs and WebSockets** for cryptocurrency market data.
We use **Twelve Data APIs and WebSockets** for cryptocurrency market data.

The architecture therefore includes both:

* historical/current market information
* real-time market monitoring

The WebSocket component is particularly relevant to threshold monitoring because Semicolon needs to react to changing prices rather than relying exclusively on periodic polling.

---

## 15. Stop-loss and take-profit monitoring

Stop-loss and take-profit functionality is also part of Semicolon's investment functionality.

Semicolon monitors configured thresholds using market data.

When the relevant threshold is reached/crossed, Semicolon can notify the user by email.

Conceptually:

```text
Market data
     ↓
Semicolon monitoring
     ↓
Threshold reached?
     │
    Yes
     ↓
Email notification
```

This is different from Colon's role.

Colon determines the algorithmic decision.

Semicolon handles the **ongoing investment monitoring and notification lifecycle**.

---

## 16. Developer APIs

Colon is also intended to become a **developer-facing API platform**.

This gives Avandsoft a second path beyond the Semicolon consumer application.

The concept is:

```text
Research
   ↓
OptiBRL
   ↓
Colon API
   ↓
External developers / companies
```

Other applications can potentially submit supported decision data to Colon and consume the resulting decision.

This means Colon can become a reusable AI decision infrastructure product rather than remaining an internal Semicolon component.

---

## 17. Research publication strategy

Our overall strategy has an interesting three-layer structure:

### Layer 1 — Research

Develop and evaluate a new decision-making algorithm.

```text
OptiBRL research
```

### Layer 2 — Infrastructure

Implement the research as a production API.

```text
Colon
```

### Layer 3 — Application

Use that infrastructure to provide an actual service to users.

```text
Semicolon
```

This is essentially:

```text
Academic research
       ↓
Research implementation
       ↓
API infrastructure
       ↓
Consumer application
```

We also intend to publish articles about the research through Avandsoft and ultimately submit the underlying work to academic journals/societies.

---

## 18. Blog / knowledge platform

Avandsoft is also being used as a publishing platform.

We have been designing the blog specifically with SEO in mind.

---

## 19. Technical stack

Our main technology stack is roughly:

| Area               | Technology                            |
| ------------------ | ------------------------------------- |
| Company            | Avandsoft                             |
| User platform      | Semicolon                             |
| Web application    | C# / ASP.NET Razor Pages              |
| Backend            | .NET                                  |
| Decision API       | Colon                                 |
| API framework      | FastAPI                               |
| ML                 | PyTorch                               |
| Algorithm          | OptiBRL                               |
| Database           | SQL Server                            |
| ORM                | Entity Framework Core                 |
| Frontend           | HTML / Bootstrap / JavaScript         |
| Crypto data        | Binance API + WebSocket               |
| Commodity data     | Twelve Data                           |
| AI text generation | LLM-based                             |
| Containerization   | Docker                                |
| Deployment         | Kubernetes / container infrastructure |
| Hosting experience | Plesk / Cloudflare                    |
| CI/CD              | GitHub Actions                        |
| Container registry | GitHub Container Registry             |

---

## 20. Backend architecture

We tend to use a relatively structured backend architecture rather than putting all logic directly in Razor Pages.

We have worked with concepts such as:

* domain models
* read models
* application services
* CQRS
* validation pipelines
* Entity Framework Core
* API services
* external authentication
* background/monitoring processes

This reflects a broader goal of keeping Semicolon as a serious backend application rather than simply a collection of Razor pages.

---

## 21. Authentication and identity

Semicolon has authentication functionality including external login.

We have worked on:

* Google authentication
* Microsoft Identity
* automatic account creation for first-time external users
* sign-in for existing users
* privacy-policy acceptance during registration

---

## 22. UI/UX

Semicolon uses **Bootstrap** heavily.

Some of the UI work we have implemented or discussed includes:

* cards
* modals
* progress interfaces
* questionnaire interfaces
* Bootstrap responsive layouts
* ApexCharts
* loading overlays
* CAPTCHA
* language selection
* decision-result interfaces

---

## 23. Investment architecture

The investment subsystem is essentially another data pipeline inside Semicolon:

```text
External market provider
          ↓
     Market data
          ↓
       Semicolon
          ↓
 ┌────────┴─────────┐
 │                  │
Historical       Real-time
data             monitoring
 │                  │
 └────────┬─────────┘
          ↓
 Investment information
          ↓
 Decision support
          ↓
 Threshold monitoring
          ↓
 Email notification
```

We have specifically worked with:

* Binance WebSocket
* cryptocurrency price/history
* take-profit
* stop-loss
* email notification
* Twelve Data commodities

---

## 24. Infrastructure

We have experience deploying and operating the platform across several environments.

You've worked with:

### Plesk

For hosting .NET applications and configuring environment variables.

### Cloudflare

For:

* DNS
* CDN
* migration
* email-related configuration
* performance optimization

### Kubernetes

We have been moving parts of the system toward Kubernetes/container-based infrastructure.

### Docker

Colon, in particular, has been packaged as a Dockerized FastAPI/PyTorch application.

A representative architecture you've worked with is:

```text
Docker
   ↓
PyTorch + Python
   ↓
FastAPI
   ↓
Colon
```

### GitHub Actions

We have a CI/CD pipeline that builds .NET applications and Docker images and publishes images to:

```text
ghcr.io
```

---

## 25. Colon deployment

Colon is designed to be lightweight in terms of its API surface.

The application loads the trained PyTorch model and exposes the decision endpoint through FastAPI.

We have preferred a **singleton-style model loading strategy**, so the model isn't repeatedly loaded for every request.

We have also considered:

* Docker
* FastAPI readiness probes
* bearer-token authorization
* environment/configuration-based secrets
* model serialization
* `.pth` models
* possible ONNX conversion

The project does not require a traditional database for the core Colon API.

---

## 26. Security and infrastructure work

We have also worked on:

* environment variables
* authentication
* Google OAuth
* security.txt
* email authentication
* SPF
* DKIM
* PTR/rDNS
* SMTP
* MailEnable

---

## 27. Performance and SEO

Performance has also been an active concern.

with resources such as:

* Bootstrap CSS
* Bootstrap Icons
* SVG resources
* third-party resources

We have also worked on:

* canonical URLs
* sitemap
* Google Analytics
* indexing
* duplicate-content problems
* redirects
* private pages
* SEO metadata
* structured blog content

The goal is not merely to make Semicolon technically functional but also discoverable through search engines.

---

## 28. Our core product philosophy

The central idea I take from all of our  descriptions is this:

**Avandsoft is trying to turn research about better decision-making into usable technology.**

The progression is not:

```text
Prediction → highest number → recommendation
```

Instead, our  research is concerned with:

```text
Uncertainty
     +
Probability
     +
Decision-making
     +
Learning
     +
Environment
     ↓
Better decision strategy
```

OptiBRL represents the research layer.

Colon represents the reusable computational/API layer.

Semicolon represents the user-facing application layer.

---

## 29. The three most important distinctions

The only three architectural facts about our ecosystem, they would be these:

### 1. Semicolon ≠ Colon

**Semicolon is the product. Colon is the decision API/engine.**

Semicolon owns the user-facing domain and decision scenarios.

Colon receives structured decision data and calculates the algorithmic result.

### 2. Questionnaires ≠ Decision Engine

Questionnaires are primarily a mechanism for:

```text
Questionnaire
    ↓
Personalized report
    +
Credits
```

They are **not inherently part of the decision recommendation pipeline**.

### 3. OptiBRL ≠ simple EV maximization

Our research is specifically motivated by the limitations of treating every uncertain decision as:

$$
a^* = \arg\max_a E[V(a)]
$$

We are investigating reinforcement-learning-based decision strategies that can perform differently in probabilistic environments over time.

That research is implemented in Colon and made useful through Semicolon.

---

## 30. Overall ecosystem

Putting everything together:

```text
                           AVANDSOFT
                    Technology + Research
                              │
            ┌─────────────────┴──────────────────┐
            │                                    │
        Research                              Products
            │                                    │
        OptiBRL                             SEMICOLON
            │                                    │
            ▼                    ┌───────────────┼────────────────┐
          COLON                  │               │                │
            │              Questionnaires   Decision Scenarios  Investment
            │                  │               │                │
       /decision               │               │          ┌─────┴─────┐
            │                  │               │          │           │
            ▼                  ▼               ▼       Crypto     Commodities
         OptiBRL         Reports + Credits     │          │           │
            │                                  │          └─────┬─────┘
            │                                  │                │
            │                                  └────────► Structured
            │                                             market data
            │                                                   │
            └───────────────────────────────────────────────────┘
                                                                │
                                                                ▼
                                                           /decision
                                                                │
                                                                ▼
                                                             OptiBRL
                                                                │
                                                                ▼
                                                         Advice / Action
```

---

## In one sentence

Avandsoft is the research and technology umbrella; OptiBRL is the reinforcement-learning decision algorithm; Colon is the FastAPI decision engine that exposes OptiBRL through `/decision`; and Semicolon is the C# user-facing platform that structures daily decisions and investment data, sends the appropriate structured inputs to Colon, presents the resulting advice, manages investment monitoring, and separately provides questionnaires, personalized reports, and credits.