<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed, nextTick } from 'vue'

// Language / Internationalization
type Locale = 'en' | 'pt'
const locale = ref<Locale>((localStorage.getItem('portfolio-locale') as Locale) || 'en')
const showLanguageSelection = ref(!localStorage.getItem('portfolio-locale'))

const translations = {
  en: {
    professionalBio: `
  Fullstack Developer with 6+ years of experience building scalable systems and
  RESTful APIs.
  Expert in the Laravel ecosystem and modern Javascript frameworks.
  My expertise lies in bridging complex backend architecture with reactive,
  high-performance frontends.
  Specialized in Dockerized environments and database optimization.
  Degree in Systems Analysis and Development from PUCPR.
`,
    languagePrompt: [
      '',
      '  ┌──────────────────────────────────┐',
      '  │      SELECT YOUR LANGUAGE     │',
      '  │      SELECIONE SEU IDIOMA     │',
      '  └──────────────────────────────────┘',
      '',
    ],
    welcome: [
      '',
      '  Welcome to my portfolio terminal!',
      '  Type a command or click the options below to navigate.',
      '',
      '  Available commands:',
      '    whoami      - About me',
      '    ls projects - View my projects',
      '    history     - Professional experience',
      '    neofetch    - System information',
      '    contact     - Get in touch',
      '',
    ],
    systemInfo: {
      os: 'Laravel 12',
      shell: 'Vue 3 + TypeScript',
      terminal: 'Tailwind CSS',
      uptime: '6+ years of experience',
      packages: '100+ npm packages mastered',
      resolution: 'Pixel Perfect',
      de: 'Clean Architecture',
      wm: 'Component-Based',
      theme: 'Dark Mode',
      cpu: 'High Performance',
      memory: 'Optimized',
    },
    projects: [
      {
        id: 'softweek',
        name: 'Softweek 2024 & 2025',
        description:
          'Official platform for the Software Engineering Academic Week (Campo Real). ' +
          'The project evolved from a seasonal solution in 2024 to a robust and scalable system in 2025. ' +
          'I implemented a complete automated checkout flow integrated with MercadoPago, ensuring real-time ' +
          'payment reconciliation for hundreds of registrations. Key highlights include a clean Laravel architecture ' +
          'and a reactive administrative interface built with Vue 3.',
        stack: ['PHP 8.2', 'Laravel 11', 'Payment Gateway', 'MySQL', 'Vue 3', 'Tailwind', 'Docker'],
        featured: true,
        github: 'https://github.com/nyannakano/softweek',
        images: [
          '/images/projects/softweek/1.png',
          '/images/projects/softweek/2.png',
          '/images/projects/softweek/3.png',
          '/images/projects/softweek/4.png',
          '/images/projects/softweek/5.png',
          '/images/projects/softweek/6.png',
          '/images/projects/softweek/7.png',
        ],
      },
      {
        id: 'noruega-realstate',
        name: 'Noruega Real Estate',
        featured: true,
        description:
          'A comprehensive digital solution custom-built for Noruega Real Estate in Curitiba. ' +
          'The project involved creating a robust property listing platform featuring an automated XML ' +
          'importer that synchronizes the database in real-time with the agency’s external CRMs. ' +
          'Developed with Laravel 12 and Vue 3 (Inertia.js), the system leverages SSR (Server-Side Rendering) ' +
          'focused on SEO to ensure maximum organic visibility on Google.',
        stack: ['PHP 8.2', 'Laravel 12', 'Vue 3', 'Inertia.js', 'MySQL', 'SSR', 'SEO', 'Tailwind', 'XML Integration', 'Docker'],
        images: [
          '/images/projects/noruega/1.png',
          '/images/projects/noruega/2.png',
          '/images/projects/noruega/3.png',
          '/images/projects/noruega/4.png',
          '/images/projects/noruega/5.png',
          '/images/projects/noruega/6.png',
          '/images/projects/noruega/7.png',
        ],
      },
      {
        id: 'MAL-bluesky',
        name: 'MAL Bluesky Integrator',
        description:
          'An automation tool developed to integrate MyAnimeList and Bluesky APIs. ' +
          'The system utilizes OAuth2 for secure authentication and data synchronization. ' +
          'I implemented a Redis caching layer to optimize MAL API consumption and ensure high performance. ' +
          'The primary focus was addressing the social integration gap following the mass user migration to Bluesky in Brazil. ' +
          'The development prioritized robust backend logic and API orchestration, while keeping the frontend ' +
          'minimalist to focus entirely on functionality and sync experience.',
        stack: ['PHP 8.1', 'Laravel 10', 'MySQL', 'OAuth2', 'Blade', 'Bootstrap', 'Docker', 'Backend-First'],
        github: 'https://github.com/nyannakano/mal-bluesky',
        images: [
          '/images/projects/mal-bluesky/1.png',
          '/images/projects/mal-bluesky/2.png',
        ],
      },
      {
        id: 'nursefy',
        name: 'Nursefy',
        description:
          'A custom-built healthcare marketplace platform connecting nurses with patients in need of professional companionship. ' +
          'I developed the backend and frontend (Laravel monolith) focusing on an agile and secure scheduling system. ' +
          'Implemented a streamlined PIX payment workflow and a shift management area for professionals. ' +
          'The project demonstrates my ability to build functional and scalable MVPs in dynamic startup environments.',
        stack: ['PHP 8.1', 'Laravel 10', 'MySQL', 'Blade', 'Tailwind', 'Docker', 'Startup MVP'],
        images: [
          '/images/projects/nursefy/1.png',
          '/images/projects/nursefy/2.png',
          '/images/projects/nursefy/3.png',
          '/images/projects/nursefy/4.png',
          '/images/projects/nursefy/5.png',
          '/images/projects/nursefy/6.png',
          '/images/projects/nursefy/7.png',
          '/images/projects/nursefy/8.png',
          '/images/projects/nursefy/9.png',
          '/images/projects/nursefy/10.png',
          '/images/projects/nursefy/11.png',
        ],
      },
      {
        id: 'tech-assessment-domains',
        name: 'Fullstack Case: Domain Management',
        description:
          'An advanced technical challenge (Laravel 12 + Next.js 15.3) developed for the Schmah Sondahl recruitment process, where I was approved and hired. ' +
          'I structured the project as a Monorepo, using Docker Compose to orchestrate separate containers for backend and frontend, ensuring both start in sync with a single command. ' +
          'Implemented an MVC architecture with a Service Layer, prioritizing readability and maintainability. ' +
          'I chose to build the Docker infrastructure from scratch (without Laravel Sail) to allow granular customizations for Nginx and PHP-FPM.',
        stack: ['PHP 8.3', 'Laravel 12', 'Next.js 15', 'Tailwind', 'Monorepo', 'Docker', 'Custom Nginx', 'Service Pattern'],
        github: 'https://github.com/nyannakano/dominios',
        images: [
          '/images/projects/domains/1.png',
          '/images/projects/domains/2.png',
          '/images/projects/domains/3.png',
          '/images/projects/domains/4.png',
        ],
      }
    ],
    experiences: [
      {
        id: 'schmah-sondahl-mid-level',
        company: 'Schmah Sondahl Portais e Sistemas',
        role: 'Mid-level Fullstack Developer',
        period: 'Jul 2025 - Dec 2025',
        location: 'Curitiba, Brazil',
        description:
          'Engineered high-performance real estate portals and complex search engine systems.',
        achievements: [
          'Developed scalable microservices and robust APIs utilizing Laravel 12 and PHP 8.4.',
          'Implemented Multi-tenant architecture for efficient management of multiple partners within a single platform.',
          'Direct integration with banking APIs for automated billing (boletos) and payment reconciliation.',
          'Created modern interfaces with Next.js 15 and orchestrated environments using Docker.'
        ],
        technologies: ['Laravel 12', 'Next.js', 'Multi-tenancy', 'Pest', 'Microservices', 'Banking APIs', 'Docker', 'Redis'],
        current: false,
      },
      {
        id: 'palinha-musical-jr',
        company: 'Palinha Musical App',
        role: 'Junior Backend Developer',
        period: 'Oct 2023 - Jun 2025',
        location: 'Guarapuava, Brazil',
        description:
          'Focused on backend infrastructure, scalability, and financial service integrations.',
        achievements: [
          'Optimized RESTful APIs to ensure seamless integration between mobile and web systems.',
          'Implemented Redis caching layers to improve system performance and scalability.',
          'Managed AWS cloud infrastructure and containerized services using Docker.',
          'Integrated payment gateways and automated tax/invoice generation systems.'
        ],
        technologies: ['Laravel', 'MySQL', 'Redis', 'REST API', 'PHPUnit', 'AWS', 'Docker', 'Payment Gateways', 'Websockets', 'Messaging'],
      },
      {
        id: 'palinha-musical-intern',
        company: 'Palinha Musical App',
        role: 'Backend Development Intern',
        period: 'Jun 2023 - Oct 2023',
        location: 'Guarapuava, Brazil',
        description: 'Career kickoff focusing on the development and maintenance of RESTful APIs for mobile and web ecosystems.',
        achievements: [
          'Assisted in developing API routes and MySQL database integrations.',
          'Implemented secure payment processing via external gateways.',
          'Ensured smooth integration between backend, mobile, and frontend systems.'
        ],
        technologies: ['Laravel', 'MySQL', 'PHP', 'REST API'],
      },
      {
        id: 'fuel-agencia',
        company: 'Fuel Agência Web',
        role: 'Fullstack Developer (Freelancer)',
        period: 'Jul 2023 - Oct 2025',
        location: 'Remote',
        description:
          'End-to-end development of custom projects, from initial conception to maintenance.',
        achievements: [
          'Designed and implemented new software solutions and APIs from scratch.',
          'Modernized and maintained legacy systems, ensuring stability and introducing new features.',
          'Worked with monolithic and microservices architectures using Laravel and Vue.js.',
          'Assisted in the development of a robust real estate system using Django.'
        ],
        technologies: ['Laravel', 'Vue.js', 'PHP', 'Blade', 'MySQL', 'Inertia.js', 'Python', 'Django'],
      },
      {
        id: 'kuba-inteligencia',
        company: 'Kuba Inteligência da Informação',
        role: 'Fullstack Developer',
        period: 'Oct 2020 - Feb 2022',
        location: 'Guarapuava, Brazil',
        description:
          'Generalist role in web applications and support for mobile development.',
        achievements: [
          'Built complete web applications and supported core features for mobile apps.',
          'Utilized PHP/Laravel and JavaScript to create dynamic, data-driven solutions.',
          'Assisted in cross-platform projects using Flutter for mobile delivery.'
        ],
        technologies: ['Laravel', 'JavaScript', 'MySQL', 'Flutter', 'PHP'],
      }
    ],
    contacts: [
      { label: 'GitHub', url: 'https://github.com/nyannakano', icon: '' },
      { label: 'LinkedIn', url: 'https://www.linkedin.com/in/gabriel-nakano/?locale=en-US', icon: '' },
      { label: 'Email', url: 'mailto:gabrielnakano123@gmail.com', icon: '' },
    ],
    ui: {
      fullStackDeveloper: 'Full Stack Developer',
      featured: 'FEATURED',
      current: 'CURRENT',
      viewOnGitHub: '→ View on GitHub',
      pressEscToClose: 'Press ESC to close',
      pressEscArrows: 'Press ESC to close | ← → to navigate images',
      close: 'close',
      view: 'view',
      github: 'github',
      details: 'details',
      more: 'more',
      systemInfo: 'System Info',
      builtWith: 'Built with Vue 3 + TypeScript + Tailwind CSS',
      foundProjects: (n: number) => `Found ${n} projects in ~/projects/`,
      clickViewScreenshots: 'Click [ view ] to see screenshots or [ github ] for source code.',
      positions: 'positions',
      yearsExperience: 'years of experience',
      clickDetails: 'Click [ details ] to see full job description and achievements.',
      interestedInOpportunities: "I'm always interested in hearing about new opportunities,",
      interestingProjects: 'interesting projects, or just connecting with fellow developers.',
      findMeAt: 'Find me at: gabrielnakano123@gmail.com',
      letsConnect: "Feel free to reach out - let's build something amazing together!",
    },
    headers: {
      whoami: [
        '┌──────────────────────────────────┐',
        '│    GABRIEL TSUTOMO NAKANO     │',
        '│     Full Stack Developer      │',
        '└──────────────────────────────────┘',
      ],
      projects: [
        '╔══════════════════════════════════╗',
        '║         MY PROJECTS           ║',
        '╚══════════════════════════════════╝',
      ],
      experience: [
        '╔══════════════════════════════════╗',
        '║   PROFESSIONAL EXPERIENCE     ║',
        '╚══════════════════════════════════╝',
      ],
      contact: [
        '┌──────────────────────────────────┐',
        '│        GET IN TOUCH           │',
        '└──────────────────────────────────┘',
      ],
    },
  },
  pt: {
    professionalBio: `
  Desenvolvedor Fullstack com mais de 6 de experiência na construção de
  sistemas escaláveis e APIs RESTful.
  Especialista no ecossistema Laravel e em frameworks modernos de Javascript.
  Minha expertise reside em conectar arquiteturas de backend complexas com
  interfaces de front-end reativas e de alta performance.
  Especializado em ambientes conteinerizados com Docker e otimização de bancos de
  dados.
  Formado em Análise e Desenvolvimento de Sistemas pela PUCPR.
`,
    languagePrompt: [
      '',
      '  ┌──────────────────────────────────┐',
      '  │      SELECT YOUR LANGUAGE     │',
      '  │      SELECIONE SEU IDIOMA     │',
      '  └──────────────────────────────────┘',
      '',
    ],
    welcome: [
      '',
      '  Bem-vindo ao meu terminal portfólio!',
      '  Digite um comando ou clique nas opções abaixo para navegar.',
      '',
      '  Comandos disponíveis:',
      '    whoami      - Sobre mim',
      '    ls projects - Ver meus projetos',
      '    history     - Experiência profissional',
      '    neofetch    - Informações do sistema',
      '    contact     - Entre em contato',
      '',
    ],
    systemInfo: {
      os: 'Laravel 12',
      shell: 'Vue 3 + TypeScript',
      terminal: 'Tailwind CSS',
      uptime: '6+ anos de experiência',
      packages: '100+ pacotes npm dominados',
      resolution: 'Pixel Perfeito',
      de: 'Arquitetura Limpa',
      wm: 'Baseado em Componentes',
      theme: 'Modo Escuro',
      cpu: 'Alta Performance',
      memory: 'Otimizado',
    },
    projects: [
      {
        id: 'softweek',
        name: 'Softweek 2024 e 2025',
        description:
          'Plataforma oficial da semana acadêmica de Engenharia de Software (Campo Real). ' +
          'O projeto evoluiu de uma solução sazonal em 2024 para um sistema robusto e escalável em 2025. ' +
          'Implementei um fluxo completo de checkout automatizado integrado ao MercadoPago, garantindo conciliação ' +
          'de pagamentos em tempo real para centenas de inscrições. Destaque para a arquitetura limpa em Laravel ' +
          'e uma interface administrativa reativa em Vue 3.',
        stack: ['PHP 8.2', 'Laravel 11', 'Gateway de Pagamento', 'MySQL', 'Vue 3', 'Tailwind', 'Docker'],
        featured: true,
        github: 'https://github.com/nyannakano/softweek',
        images: [
          '/images/projects/softweek/1.png',
          '/images/projects/softweek/2.png',
          '/images/projects/softweek/3.png',
          '/images/projects/softweek/4.png',
          '/images/projects/softweek/5.png',
          '/images/projects/softweek/6.png',
          '/images/projects/softweek/7.png',
        ],
      },
      {
        id: 'noruega-realstate',
        name: 'Imobiliária Noruega',
        featured: true,
        description:
          'Solução digital completa desenvolvida sob encomenda para a Imobiliária Noruega, em Curitiba. ' +
          'O projeto consistiu na criação de uma plataforma de listagem de imóveis robusta, ' +
          'onde implementei um importador de XML automatizado que sincroniza o banco de dados em tempo real com os CRMs utilizados pela empresa. ' +
          'Desenvolvido com Laravel 12 e Vue 3 (Inertia.js), o sistema utiliza SSR (Server-Side Rendering) ' +
          'focado em SEO para garantir que a imobiliária tenha máxima visibilidade orgânica no Google.',
        stack: ['PHP 8.2', 'Laravel 12', 'Vue 3', 'Inertia.js', 'MySQL', 'SSR', 'SEO', 'Tailwind', 'XML Integration', 'Docker',],
        images: [
          '/images/projects/noruega/1.png',
          '/images/projects/noruega/2.png',
          '/images/projects/noruega/3.png',
          '/images/projects/noruega/4.png',
          '/images/projects/noruega/5.png',
          '/images/projects/noruega/6.png',
          '/images/projects/noruega/7.png',
        ],
      },
      {
        id: 'MAL-bluesky',
        name: 'MAL Bluesky',
        description:
          'Ferramenta de automação desenvolvida para integrar as APIs do MyAnimeList e Bluesky. ' +
          'O sistema utiliza OAuth2 para autenticação segura e sincronização de dados. ' +
          'Implementei uma camada de cache com Redis para otimizar o consumo da API do MAL e garantir performance. ' +
          'O foco principal foi resolver a lacuna de integrações sociais após a migração em massa de usuários para o Bluesky no Brasil. ' +
          'O desenvolvimento priorizou a robustez da lógica de backend e a orquestração de APIs, ' +
          'com o front-end mantido minimalista para foco total na funcionalidade e experiência de sincronização. ',
        stack: ['PHP 8.1', 'Laravel 10', 'MySQL', 'OAuth2', 'Blade', 'Bootstrap', 'Docker', 'Backend-First'],
        github: 'https://github.com/nyannakano/mal-bluesky',
        images: [
          '/images/projects/mal-bluesky/1.png',
          '/images/projects/mal-bluesky/2.png',
        ],
      },
      {
        id: 'nursefy',
        name: 'Nursefy',
        description:
          'Plataforma de marketplace voltada à saúde desenvolvida sob encomenda, conectando enfermeiros a pacientes que necessitavam de acompanhamento hospitalar. ' +
          'Desenvolvi o backend e frontend (monolito Laravel) focando em um sistema de agendamento ágil e seguro. ' +
          'Implementei um fluxo de pagamento simplificado via PIX e uma área de gestão de turnos para os profissionais. ' +
          'O projeto demonstra minha capacidade de criar MVPs funcionais e escaláveis em ambientes dinâmicos de startup.',
        stack: ['PHP 8.1', 'Laravel 10', 'MySQL', 'Blade', 'Tailwind', 'Docker', 'Startup MVP'],
        images: [
          '/images/projects/nursefy/1.png',
          '/images/projects/nursefy/2.png',
          '/images/projects/nursefy/3.png',
          '/images/projects/nursefy/4.png',
          '/images/projects/nursefy/5.png',
          '/images/projects/nursefy/6.png',
          '/images/projects/nursefy/7.png',
          '/images/projects/nursefy/8.png',
          '/images/projects/nursefy/9.png',
          '/images/projects/nursefy/10.png',
          '/images/projects/nursefy/11.png',
        ],
      },
      {
        id: 'tech-assessment-domains',
        name: 'Fullstack Case: Domain Management',
        description:
          'Desafio técnico avançado (Laravel 12 + Next.js 15.3) desenvolvido para o processo seletivo da Schmah Sondahl, onde fui aprovado e contratado. ' +
          'Estruturei o projeto como um Monorepo, utilizando Docker Compose para orquestrar containers distintos para o back-end e front-end, garantindo que ambos iniciem em sincronia com um único comando. ' +
          'Implementei uma arquitetura MVC com Service Layer, priorizando a legibilidade e manutenção. ' +
          'Optei por construir a infraestrutura Docker do zero (sem Laravel Sail) para permitir customizações granulares no Nginx e PHP-FPM.',
        stack: ['PHP 8.3', 'Laravel 12', 'Next.js 15', 'Tailwind', 'Monorepo', 'Docker', 'Custom Nginx', 'Service Pattern'],
        github: 'https://github.com/nyannakano/dominios',
        images: [
          '/images/projects/domains/1.png',
          '/images/projects/domains/2.png',
          '/images/projects/domains/3.png',
          '/images/projects/domains/4.png',
        ],
      }
    ],
    experiences: [
      {
        id: 'schmah-sondahl-mid-level',
        company: 'Schmah Sondahl Portais e Sistemas',
        role: 'Desenvolvedor Fullstack Pleno',
        period: 'Jul 2025 - Dez 2025',
        location: 'Curitiba, Brasil',
        description:
          'Desenvolvimento de portais imobiliários de alta performance e sistemas de busca complexos.',
        achievements: [
          'Desenvolvimento de microsserviços escaláveis e APIs robustas utilizando Laravel 12 e PHP 8.4.',
          'Implementação de arquitetura Multi-tenant para gestão eficiente de múltiplos parceiros em uma única plataforma.',
          'Integração direta com APIs bancárias para automação de geração e conciliação de boletos.',
          'Criação de interfaces modernas com Next.js 15 e orquestração de containers com Docker.'
        ],
        technologies: ['Laravel 12', 'Next.js', 'Multi-tenancy', 'Pest', 'Microsserviços', 'APIs Bancárias', 'Docker', 'Redis'],
        current: false,
      },
      {
        id: 'palinha-musical-jr',
        company: 'Palinha Musical App',
        role: 'Desenvolvedor Backend Júnior',
        period: 'Out 2023 - Jun 2025',
        location: 'Guarapuava, Brasil',
        description:
          'Foco total em infraestrutura de backend, escalabilidade e integração de serviços financeiros.',
        achievements: [
          'Otimização de APIs RESTful com integração entre sistemas mobile e web.',
          'Implementação de camadas de cache com Redis para melhoria de performance e escalabilidade.',
          'Gerenciamento de infraestrutura em nuvem AWS e conteinerização com Docker.',
          'Integração de gateways de pagamento e sistemas de emissão de notas fiscais.'
        ],
        technologies: ['Laravel', 'MySQL', 'Redis', 'API REST', 'PHPUnit', 'AWS', 'Docker', 'Gateways de Pagamento', 'Websockets', 'Mensageria'],
      },
      {
        id: 'palinha-musical-intern',
        company: 'Palinha Musical App',
        role: 'Estagiário de Desenvolvimento Backend',
        period: 'Jun 2023 - Out 2023',
        location: 'Guarapuava, Brasil',
        description: 'Início da trajetória com foco no desenvolvimento e manutenção de APIs RESTful para os ecossistemas mobile e web.',
        achievements: [
          'Auxílio no desenvolvimento de rotas de API e integração com banco de dados MySQL.',
          'Implementação de processamento seguro de pagamentos via gateway.',
          'Garantia de integração fluida entre sistemas backend, mobile e frontend.'
        ],
        technologies: ['Laravel', 'MySQL', 'PHP', 'API REST'],
      },
      {
        id: 'fuel-agencia',
        company: 'Fuel Agência Web',
        role: 'Desenvolvedor Fullstack (Freelancer)',
        period: 'Jul 2023 - Out 2025',
        location: 'Remoto',
        description:
          'Desenvolvimento end-to-end de projetos customizados, desde a concepção até a manutenção.',
        achievements: [
          'Projetei e implementei novas soluções de software e APIs partindo do zero.',
          'Evolução e manutenção de sistemas legados, garantindo estabilidade e novas features.',
          'Trabalhei com arquiteturas monolíticas e microsserviços usando Laravel e Vue.js.',
          'Auxiliei no desenvolvimento de sistema robusto do ramo imobiliário utilizando Django.'
        ],
        technologies: ['Laravel', 'Vue.js', 'PHP', 'Blade', 'MySQL', 'Vue', 'Inertia.js', 'Python', 'Django'],
      },
      {
        id: 'kuba-inteligencia',
        company: 'Kuba Inteligência da Informação',
        role: 'Desenvolvedor Fullstack',
        period: 'Out 2020 - Fev 2022',
        location: 'Guarapuava, Brasil',
        description:
          'Atuação generalista em aplicações web e suporte ao desenvolvimento mobile.',
        achievements: [
          'Desenvolvimento de aplicações completas e suporte a funcionalidades em apps mobile.',
          'Utilização de PHP/Laravel e JavaScript para criação de soluções dinâmicas.',
          'Apoio em projetos utilizando Flutter para entregas cross-platform.'
        ],
        technologies: ['Laravel', 'JavaScript', 'MySQL', 'Flutter', 'PHP'],
      }
    ],
    contacts: [
      { label: 'GitHub', url: 'https://github.com/nyannakano', icon: '' },
      { label: 'LinkedIn', url: 'https://www.linkedin.com/in/gabriel-nakano', icon: '' },
      { label: 'Email', url: 'mailto:gabrielnakano123@gmail.com', icon: '' },
    ],
    ui: {
      fullStackDeveloper: 'Desenvolvedor Full Stack',
      featured: 'DESTAQUE',
      current: 'ATUAL',
      viewOnGitHub: '→ Ver no GitHub',
      pressEscToClose: 'Pressione ESC para fechar',
      pressEscArrows: 'Pressione ESC para fechar | ← → para navegar imagens',
      close: 'fechar',
      view: 'ver',
      github: 'github',
      details: 'detalhes',
      more: 'mais',
      systemInfo: 'Info do Sistema',
      builtWith: 'Feito com Vue 3 + TypeScript + Tailwind CSS',
      foundProjects: (n: number) => `Encontrados ${n} projetos em ~/projects/`,
      clickViewScreenshots: 'Clique [ ver ] para screenshots ou [ github ] para código fonte.',
      positions: 'posições',
      yearsExperience: 'anos de experiência',
      clickDetails: 'Clique [ detalhes ] para ver descrição completa e conquistas.',
      interestedInOpportunities: 'Estou sempre interessado em ouvir sobre novas oportunidades,',
      interestingProjects: 'projetos interessantes, ou apenas conectar com outros desenvolvedores.',
      findMeAt: 'Me encontre em: gabrielnakano123@gmail.com',
      letsConnect: 'Sinta-se à vontade para entrar em contato - vamos construir algo incrível juntos!',
    },
    headers: {
      whoami: [
        '┌──────────────────────────────────┐',
        '│    GABRIEL TSUTOMO NAKANO     │',
        '│   Desenvolvedor Full Stack    │',
        '└──────────────────────────────────┘',
      ],
      projects: [
        '╔══════════════════════════════════╗',
        '║        MEUS PROJETOS          ║',
        '╚══════════════════════════════════╝',
      ],
      experience: [
        '╔══════════════════════════════════╗',
        '║  EXPERIÊNCIA PROFISSIONAL     ║',
        '╚══════════════════════════════════╝',
      ],
      contact: [
        '┌──────────────────────────────────┐',
        '│      ENTRE EM CONTATO         │',
        '└──────────────────────────────────┘',
      ],
    },
  },
}

// Computed translation helper
const t = computed(() => translations[locale.value])

function selectLanguage(lang: Locale): void {
  locale.value = lang
  localStorage.setItem('portfolio-locale', lang)
  showLanguageSelection.value = false
  showWelcome()
}

// Type definitions
interface Project {
  id: string
  name: string
  description: string
  stack: string[]
  featured?: boolean
  github?: string
  images?: string[]
}
interface Experience {
  id: string
  company: string
  role: string
  period: string
  location: string
  description: string
  achievements: string[]
  technologies: string[]
  current?: boolean
}

// ============================================================================
// TERMINAL LOGIC
// ============================================================================

// Boot sequence messages
const bootMessages = [
  '[    0.000000] Linux version 6.1.0-nakano-dev (gcc 13.2.0)',
  '[    0.000001] Command line: BOOT_IMAGE=/vmlinuz-nakano root=/dev/sda1',
  '[    0.000002] BIOS-provided physical RAM map:',
  '[    0.000003] BIOS-e820: [mem 0x0000000000000000-0x000000000009fbff] usable',
  '[    0.000004] Initializing cgroup subsys cpuset',
  '[    0.000005] Initializing cgroup subsys cpu',
  '[    0.000008] CPU: x86_64 Architecture detected',
  '[    0.000012] Loading portfolio modules...',
  '[    0.000015] [OK] vue-3.5.x loaded',
  '[    0.000018] [OK] typescript-5.x loaded',
  '[    0.000021] [OK] tailwindcss-4.x loaded',
  '[    0.000024] [OK] composition-api initialized',
  '[    0.000027] Mounting developer filesystem...',
  '[    0.000030] [OK] /home/gabriel mounted',
  '[    0.000033] [OK] /projects mounted',
  '[    0.000036] [OK] /skills mounted',
  '[    0.000039] Starting network services...',
  '[    0.000042] [OK] HTTP/3 enabled',
  '[    0.000045] [OK] WebSocket connections ready',
  '[    0.000048] System ready. Loading terminal interface...',
  '',
]

const asciiArt = `
███╗   ██╗ █████╗ ██╗  ██╗ █████╗ ███╗   ██╗ ██████╗
████╗  ██║██╔══██╗██║ ██╔╝██╔══██╗████╗  ██║██╔═══██╗
██╔██╗ ██║███████║█████╔╝ ███████║██╔██╗ ██║██║   ██║
██║╚██╗██║██╔══██║██╔═██╗ ██╔══██║██║╚██╗██║██║   ██║
██║ ╚████║██║  ██║██║  ██╗██║  ██║██║ ╚████║╚██████╔╝
╚═╝  ╚═══╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═══╝ ╚═════╝
`

// State management
const isBooting = ref(true)
const bootProgress = ref<string[]>([])
const showAscii = ref(false)
const showTerminal = ref(false)
const currentSection = ref<'home' | 'whoami' | 'projects' | 'experience' | 'contact'>('home')
const terminalOutput = ref<string[]>([])
const isTyping = ref(false)
const terminalRef = ref<HTMLElement | null>(null)

// Project Modal state
const showModal = ref(false)
const selectedProject = ref<Project | null>(null)
const currentImageIndex = ref(0)

// Experience Modal state
const showExperienceModal = ref(false)
const selectedExperience = ref<Experience | null>(null)

function openProjectModal(project: Project): void {
  selectedProject.value = project
  currentImageIndex.value = 0
  showModal.value = true
}

function closeModal(): void {
  showModal.value = false
  selectedProject.value = null
  currentImageIndex.value = 0
}

function openExperienceModal(experience: Experience): void {
  selectedExperience.value = experience
  showExperienceModal.value = true
}

function closeExperienceModal(): void {
  showExperienceModal.value = false
  selectedExperience.value = null
}

function nextImage(): void {
  if (selectedProject.value?.images) {
    currentImageIndex.value = (currentImageIndex.value + 1) % selectedProject.value.images.length
  }
}

function prevImage(): void {
  if (selectedProject.value?.images) {
    currentImageIndex.value =
      (currentImageIndex.value - 1 + selectedProject.value.images.length) %
      selectedProject.value.images.length
  }
}

function handleKeydown(e: KeyboardEvent): void {
  if (e.key === 'Escape') {
    if (showModal.value) closeModal()
    if (showExperienceModal.value) closeExperienceModal()
  }
  if (!showModal.value) return
  if (e.key === 'ArrowRight') nextImage()
  if (e.key === 'ArrowLeft') prevImage()
}

// Add keyboard listener on mount
onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
  runBootSequence()
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})

// Typewriter effect
async function typeText(text: string, speed = 20): Promise<void> {
  isTyping.value = true
  const lines = text.split('\n')

  for (const line of lines) {
    let currentLine = ''
    for (const char of line) {
      currentLine += char
      terminalOutput.value[terminalOutput.value.length - 1] = currentLine
      await sleep(speed)
    }
    terminalOutput.value.push('')
    await scrollToBottom()
  }

  isTyping.value = false
}

async function typeLines(lines: string[], speed = 15): Promise<void> {
  isTyping.value = true

  for (const line of lines) {
    terminalOutput.value.push('')
    let currentLine = ''
    for (const char of line) {
      currentLine += char
      terminalOutput.value[terminalOutput.value.length - 1] = currentLine
      await sleep(speed)
    }
    await scrollToBottom()
  }

  isTyping.value = false
}

function sleep(ms: number): Promise<void> {
  return new Promise((resolve) => setTimeout(resolve, ms))
}

async function scrollToBottom(): Promise<void> {
  await nextTick()
  if (terminalRef.value) {
    terminalRef.value.scrollTop = terminalRef.value.scrollHeight
  }
}

// Boot sequence
async function runBootSequence(): Promise<void> {
  for (const message of bootMessages) {
    bootProgress.value.push(message)
    await sleep(40)
  }

  await sleep(300)
  showAscii.value = true
  await sleep(1500)
  isBooting.value = false
  showTerminal.value = true

  await sleep(200)

  // If no language preference, show language selection
  if (showLanguageSelection.value) {
    terminalOutput.value = []
    await typeLines(t.value.languagePrompt, 10)
  } else {
    await showWelcome()
  }
}

// Terminal commands
async function showWelcome(): Promise<void> {
  terminalOutput.value = []
  await typeLines(t.value.welcome, 10)
  currentSection.value = 'home'
}

async function runCommand(command: string): Promise<void> {
  if (isTyping.value) return

  terminalOutput.value.push('')
  terminalOutput.value.push(`$ ${command}`)
  await sleep(100)

  switch (command) {
    case 'whoami':
      await showWhoami()
      break
    case 'ls projects':
    case 'ls -projects':
      await showProjects()
      break
    case 'history':
      await showExperience()
      break
    case 'neofetch':
      await showNeofetch()
      break
    case 'contact':
      await showContact()
      break
    case 'clear':
      terminalOutput.value = []
      currentSection.value = 'home'
      break
    case 'home':
      await showWelcome()
      break
    default:
      terminalOutput.value.push(`command not found: ${command}`)
  }

  await scrollToBottom()
}

async function showWhoami(): Promise<void> {
  currentSection.value = 'whoami'
  const lines = [
    '',
    ...t.value.headers.whoami,
    '',
    ...t.value.professionalBio
      .trim()
      .split('\n')
      .map((l: string) => `  ${l.trim()}`),
    '',
  ]
  await typeLines(lines, 8)
}

async function showProjects(): Promise<void> {
  currentSection.value = 'projects'
  const lines = [
    '',
    ...t.value.headers.projects,
    '',
    `  ${t.value.ui.foundProjects(t.value.projects.length)}`,
    '',
    `  ${t.value.ui.clickViewScreenshots}`,
    '',
  ]

  await typeLines(lines, 10)
}

async function showExperience(): Promise<void> {
  currentSection.value = 'experience'
  const exps = t.value.experiences
  const totalYears = exps.reduce((acc: number, exp: Experience) => {
    const parts = exp.period.split(' - ')
    const startStr = parts[0] ?? ''
    const endStr = parts[1] ?? ''
    const start = new Date(startStr)
    const end = (endStr === 'Present' || endStr === 'Presente') ? new Date() : new Date(endStr)
    return acc + (end.getFullYear() - start.getFullYear())
  }, 0)

  const lines = [
    '',
    ...t.value.headers.experience,
    '',
    `  ${exps.length} ${t.value.ui.positions} | ~${totalYears}+ ${t.value.ui.yearsExperience}`,
    '',
    `  ${t.value.ui.clickDetails}`,
    '',
  ]

  await typeLines(lines, 10)
}

async function showNeofetch(): Promise<void> {
  const sysInfo = t.value.systemInfo
  const neofetchArt = [
    '',
    '                    gabriel@nakano-dev',
    '        .--.        ─────────────────────',
    '       |o_o |       OS: ' + sysInfo.os,
    '       |:_/ |       Shell: ' + sysInfo.shell,
    '      //   \\ \\      Terminal: ' + sysInfo.terminal,
    '     (|     | )     Uptime: ' + sysInfo.uptime,
    '    /\\_)   (_/\\    Packages: ' + sysInfo.packages,
    '    \\___)=(___/    Resolution: ' + sysInfo.resolution,
    '                    DE: ' + sysInfo.de,
    '                    WM: ' + sysInfo.wm,
    '                    Theme: ' + sysInfo.theme,
    '                    CPU: ' + sysInfo.cpu,
    '                    Memory: ' + sysInfo.memory,
    '',
    '    ███████████████████████████████',
    '',
  ]
  await typeLines(neofetchArt, 15)
}

async function showContact(): Promise<void> {
  currentSection.value = 'contact'
  const lines = [
    '',
    ...t.value.headers.contact,
    '',
    `  ${t.value.ui.interestedInOpportunities}`,
    `  ${t.value.ui.interestingProjects}`,
    '',
    `  ${t.value.ui.letsConnect}`,
    '',
  ]

  await typeLines(lines, 10)
}

// Computed
const prompt = computed(() => 'gabriel@nakano-dev:~$')
</script>

<template>
  <div class="min-h-screen flex items-center justify-center p-4 font-mono crt-flicker">
    <!-- Scanlines overlay -->
    <div class="scanlines"></div>

    <!-- Boot Sequence -->
    <Transition name="fade">
      <div
        v-if="isBooting"
        class="fixed inset-0 bg-terminal-bg flex flex-col items-center justify-center p-8 z-50"
      >
        <!-- Boot messages -->
        <div
          v-if="!showAscii"
          class="w-full max-w-4xl h-96 overflow-hidden text-terminal-green text-xs leading-tight"
        >
          <div v-for="(msg, i) in bootProgress" :key="i" class="whitespace-pre">
            {{ msg }}
          </div>
        </div>

        <!-- ASCII Art -->
        <Transition name="scale">
          <div v-if="showAscii" class="text-center">
            <pre class="text-terminal-green text-xs sm:text-sm md:text-base glow-text leading-none">{{
              asciiArt
            }}</pre>
            <p class="text-terminal-cyan text-xl mt-4 glow-text">Gabriel Tsutomo Nakano</p>
            <p class="text-terminal-muted text-sm mt-2">{{ t.ui.fullStackDeveloper }}</p>
          </div>
        </Transition>
      </div>
    </Transition>

    <!-- Main Terminal -->
    <Transition name="slide-up">
      <div v-if="showTerminal" class="w-full max-w-5xl terminal-window">
        <!-- Terminal Header -->
        <div class="terminal-header">
          <div class="traffic-lights">
            <span class="traffic-light traffic-light--red"></span>
            <span class="traffic-light traffic-light--yellow"></span>
            <span class="traffic-light traffic-light--green"></span>
          </div>
          <span class="text-terminal-muted text-sm flex-1 text-center">
            gabriel@nakano-dev: ~
          </span>
          <div class="w-14"></div>
        </div>

        <!-- Terminal Body -->
        <div class="flex flex-col lg:flex-row">
          <!-- Neofetch Sidebar -->
          <div class="hidden lg:block w-72 p-4 border-r border-terminal bg-terminal-header/50">
            <div class="text-xs space-y-1">
              <div class="text-terminal-cyan font-bold mb-2">┌── {{ t.ui.systemInfo }} ──┐</div>
              <div>
                <span class="text-terminal-purple">OS:</span>
                <span class="text-terminal-text ml-2">{{ t.systemInfo.os }}</span>
              </div>
              <div>
                <span class="text-terminal-purple">Shell:</span>
                <span class="text-terminal-text ml-2">{{ t.systemInfo.shell }}</span>
              </div>
              <div>
                <span class="text-terminal-purple">Terminal:</span>
                <span class="text-terminal-text ml-2">{{ t.systemInfo.terminal }}</span>
              </div>
              <div>
                <span class="text-terminal-purple">Uptime:</span>
                <span class="text-terminal-text ml-2">{{ t.systemInfo.uptime }}</span>
              </div>
              <div>
                <span class="text-terminal-purple">DE:</span>
                <span class="text-terminal-text ml-2">{{ t.systemInfo.de }}</span>
              </div>
              <div>
                <span class="text-terminal-purple">Theme:</span>
                <span class="text-terminal-text ml-2">{{ t.systemInfo.theme }}</span>
              </div>
              <div class="text-terminal-cyan font-bold mt-4 mb-2">└───────────────────────┘</div>

              <!-- Color palette -->
              <div class="flex gap-1 mt-4">
                <span class="w-4 h-4 bg-terminal-red rounded-sm"></span>
                <span class="w-4 h-4 bg-terminal-yellow rounded-sm"></span>
                <span class="w-4 h-4 bg-terminal-green rounded-sm"></span>
                <span class="w-4 h-4 bg-terminal-cyan rounded-sm"></span>
                <span class="w-4 h-4 bg-terminal-blue rounded-sm"></span>
                <span class="w-4 h-4 bg-terminal-purple rounded-sm"></span>
              </div>
            </div>
          </div>

          <!-- Main Content Area -->
          <div class="flex-1 flex flex-col">
            <!-- Terminal Output -->
            <div
              ref="terminalRef"
              class="flex-1 p-4 min-h-[400px] max-h-[500px] overflow-y-auto text-sm text-terminal-text"
            >
              <div v-for="(line, i) in terminalOutput" :key="i" class="whitespace-pre-wrap">
                <span v-if="line.startsWith('$')" class="text-terminal-green">{{ line }}</span>
                <span v-else-if="line.includes('GABRIEL')" class="text-terminal-cyan font-bold">{{
                  line
                }}</span>
                <span v-else-if="line.includes('★')" class="text-terminal-yellow">{{ line }}</span>
                <span v-else-if="line.startsWith('│') || line.startsWith('┌') || line.startsWith('└') || line.startsWith('├') || line.startsWith('╔') || line.startsWith('╚') || line.startsWith('║')" class="text-terminal-muted">{{ line }}</span>
                <span v-else-if="line.includes('Stack:')" class="text-terminal-purple">{{
                  line
                }}</span>
                <span v-else-if="line.startsWith('    →')" class="text-terminal-blue">{{
                  line
                }}</span>
                <span v-else>{{ line }}</span>
              </div>

              <!-- Project Cards (shown after typing completes) -->
              <div v-if="currentSection === 'projects' && !isTyping" class="mt-4 space-y-3">
                <div
                  v-for="project in t.projects"
                  :key="project.id"
                  class="border border-terminal rounded p-3 bg-terminal-header/30 hover:bg-terminal-header/50 transition-colors"
                >
                  <div class="flex items-start justify-between gap-3">
                    <div class="flex-1 min-w-0">
                      <div class="flex items-center gap-2">
                        <span class="text-terminal-cyan font-bold">{{ project.name }}</span>
                        <span v-if="project.featured" class="text-terminal-yellow text-xs">★ {{ t.ui.featured }}</span>
                      </div>
                      <p class="text-terminal-muted text-xs mt-1">{{ project.description }}</p>
                      <div class="flex flex-wrap gap-1 mt-2">
                        <span
                          v-for="tech in project.stack"
                          :key="tech"
                          class="text-xs px-2 py-0.5 bg-terminal-purple/20 text-terminal-purple rounded"
                        >
                          {{ tech }}
                        </span>
                      </div>
                    </div>
                    <div class="flex flex-col gap-2 shrink-0 notranslate" translate="no">
                      <button
                        v-if="project.images && project.images.length > 0"
                        class="command-btn text-xs !px-3 !py-1"
                        @click="openProjectModal(project)"
                      >
                        [ {{ t.ui.view }} ]
                      </button>
                      <a
                        v-if="project.github"
                        :href="project.github"
                        target="_blank"
                        rel="noopener noreferrer"
                        class="command-btn text-xs !px-3 !py-1 text-center no-underline"
                      >
                        [ {{ t.ui.github }} ]
                      </a>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Experience Cards (shown after typing completes) -->
              <div v-if="currentSection === 'experience' && !isTyping" class="mt-4 space-y-3">
                <div
                  v-for="experience in t.experiences"
                  :key="experience.id"
                  class="border border-terminal rounded p-3 bg-terminal-header/30 hover:bg-terminal-header/50 transition-colors"
                >
                  <div class="flex items-start justify-between gap-3">
                    <div class="flex-1 min-w-0">
                      <div class="flex items-center gap-2 flex-wrap">
                        <span class="text-terminal-cyan font-bold">{{ experience.role }}</span>
                        <span v-if="experience.current" class="text-terminal-green text-xs px-2 py-0.5 bg-terminal-green/20 rounded">● {{ t.ui.current }}</span>
                      </div>
                      <div class="text-terminal-yellow text-sm mt-1">{{ experience.company }}</div>
                      <div class="text-terminal-muted text-xs mt-1">
                        {{ experience.period }} · {{ experience.location }}
                      </div>
                      <p class="text-terminal-text text-xs mt-2">{{ experience.description }}</p>
                      <div class="flex flex-wrap gap-1 mt-2">
                        <span
                          v-for="tech in experience.technologies.slice(0, 4)"
                          :key="tech"
                          class="text-xs px-2 py-0.5 bg-terminal-blue/20 text-terminal-blue rounded"
                        >
                          {{ tech }}
                        </span>
                        <span
                          v-if="experience.technologies.length > 4"
                          class="text-xs px-2 py-0.5 text-terminal-muted"
                        >
                          +{{ experience.technologies.length - 4 }} {{ t.ui.more }}
                        </span>
                      </div>
                    </div>
                    <div class="flex flex-col gap-2 shrink-0 notranslate" translate="no">
                      <button
                        class="command-btn text-xs !px-3 !py-1"
                        @click="openExperienceModal(experience)"
                      >
                        [ {{ t.ui.details }} ]
                      </button>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Contact Links (shown after typing completes) -->
              <div v-if="currentSection === 'contact' && !isTyping" class="mt-4 space-y-2 notranslate" translate="no">
                <a
                  v-for="contact in t.contacts"
                  :key="contact.label"
                  :href="contact.url"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="flex items-center gap-3 p-3 border border-terminal rounded bg-terminal-header/30 hover:bg-terminal-header/50 hover:border-terminal-cyan transition-colors group"
                >
                  <span class="text-terminal-green">→</span>
                  <span class="text-terminal-cyan group-hover:text-terminal-text transition-colors">{{ contact.label }}</span>
                  <span class="text-terminal-muted text-xs truncate">{{ contact.url }}</span>
                </a>
              </div>

              <!-- Language Selection Buttons -->
              <div v-if="showLanguageSelection && !isTyping" class="mt-4 flex justify-center gap-4 notranslate" translate="no">
                <button
                  class="command-btn !px-6 !py-3 text-base"
                  @click="selectLanguage('en')"
                >
                  [ English ]
                </button>
                <button
                  class="command-btn !px-6 !py-3 text-base"
                  @click="selectLanguage('pt')"
                >
                  [ Português ]
                </button>
              </div>

              <!-- Cursor -->
              <div v-if="!showLanguageSelection" class="flex items-center mt-2">
                <span class="text-terminal-green">{{ prompt }}</span>
                <span class="ml-2 w-2 h-4 bg-terminal-green cursor-blink"></span>
              </div>
            </div>

            <!-- Command Buttons -->
            <div v-if="!showLanguageSelection" class="p-4 border-t border-terminal bg-terminal-header/30" translate="no">
              <div class="flex flex-wrap gap-2 notranslate">
                <button
                  class="command-btn"
                  :disabled="isTyping"
                  @click="runCommand('whoami')"
                >
                  [ whoami ]
                </button>
                <button
                  class="command-btn"
                  :disabled="isTyping"
                  @click="runCommand('ls projects')"
                >
                  [ ls projects ]
                </button>
                <button
                  class="command-btn"
                  :disabled="isTyping"
                  @click="runCommand('history')"
                >
                  [ history ]
                </button>
                <button
                  class="command-btn"
                  :disabled="isTyping"
                  @click="runCommand('neofetch')"
                >
                  [ neofetch ]
                </button>
                <button
                  class="command-btn"
                  :disabled="isTyping"
                  @click="runCommand('contact')"
                >
                  [ contact ]
                </button>
                <button
                  class="command-btn"
                  :disabled="isTyping"
                  @click="runCommand('clear')"
                >
                  [ clear ]
                </button>
                <button
                  class="command-btn"
                  :disabled="isTyping"
                  @click="runCommand('home')"
                >
                  [ home ]
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- Footer -->
        <div class="p-3 border-t border-terminal text-center text-xs text-terminal-muted">
          <span>&copy; {{ new Date().getFullYear() }} Gabriel Tsutomo Nakano</span>
          <span class="mx-2">|</span>
          <span>{{ t.ui.builtWith }}</span>
        </div>
      </div>
    </Transition>

    <!-- Project Modal -->
    <Transition name="fade">
      <div
        v-if="showModal && selectedProject"
        class="fixed inset-0 z-50 flex items-center justify-center p-4"
        @click.self="closeModal"
      >
        <!-- Backdrop -->
        <div class="absolute inset-0 bg-black/80 backdrop-blur-sm"></div>

        <!-- Modal Content -->
        <div class="relative terminal-window w-full max-w-3xl max-h-[90vh] overflow-hidden">
          <!-- Modal Header -->
          <div class="terminal-header">
            <div class="traffic-lights">
              <button class="traffic-light traffic-light--red" @click="closeModal"></button>
              <span class="traffic-light traffic-light--yellow"></span>
              <span class="traffic-light traffic-light--green"></span>
            </div>
            <span class="text-terminal-muted text-sm flex-1 text-center">
              cat ./projects/{{ selectedProject.id }}/README.md
            </span>
            <div class="w-14"></div>
          </div>

          <!-- Modal Body -->
          <div class="p-4 overflow-y-auto max-h-[calc(90vh-120px)]">
            <!-- Project Title -->
            <div class="mb-4">
              <div class="flex items-center gap-2">
                <h2 class="text-terminal-cyan text-lg font-bold">{{ selectedProject.name }}</h2>
                <span v-if="selectedProject.featured" class="text-terminal-yellow text-sm">★ {{ t.ui.featured }}</span>
              </div>
              <p class="text-terminal-text text-sm mt-2">{{ selectedProject.description }}</p>

              <!-- Stack -->
              <div class="flex flex-wrap gap-1 mt-3">
                <span
                  v-for="tech in selectedProject.stack"
                  :key="tech"
                  class="text-xs px-2 py-1 bg-terminal-purple/20 text-terminal-purple rounded"
                >
                  {{ tech }}
                </span>
              </div>

              <!-- GitHub Link -->
              <a
                v-if="selectedProject.github"
                :href="selectedProject.github"
                target="_blank"
                rel="noopener noreferrer"
                class="inline-flex items-center gap-2 mt-3 text-terminal-blue hover:text-terminal-cyan transition-colors text-sm"
              >
                <span>{{ t.ui.viewOnGitHub }}</span>
              </a>
            </div>

            <!-- Image Gallery -->
            <div v-if="selectedProject.images && selectedProject.images.length > 0" class="mt-4">
              <div class="text-terminal-muted text-xs mb-2">
                $ ls ./screenshots/ | head -n 1
              </div>

              <!-- Image Container -->
              <div class="relative bg-terminal-header rounded border border-terminal overflow-hidden">
                <img
                  :src="selectedProject.images[currentImageIndex]"
                  :alt="`${selectedProject.name} screenshot ${currentImageIndex + 1}`"
                  class="w-full h-auto max-h-[400px] object-contain"
                  @error="($event.target as HTMLImageElement).src = 'https://placehold.co/800x450/0d1117/3fb950?text=Screenshot+' + (currentImageIndex + 1)"
                />

                <!-- Navigation Arrows -->
                <div v-if="selectedProject.images.length > 1" class="absolute inset-y-0 left-0 flex items-center">
                  <button
                    class="p-2 bg-terminal-bg/80 text-terminal-green hover:bg-terminal-bg transition-colors"
                    @click="prevImage"
                  >
                    ◀
                  </button>
                </div>
                <div v-if="selectedProject.images.length > 1" class="absolute inset-y-0 right-0 flex items-center">
                  <button
                    class="p-2 bg-terminal-bg/80 text-terminal-green hover:bg-terminal-bg transition-colors"
                    @click="nextImage"
                  >
                    ▶
                  </button>
                </div>
              </div>

              <!-- Image Counter -->
              <div v-if="selectedProject.images.length > 1" class="flex justify-center gap-2 mt-3">
                <button
                  v-for="(_, idx) in selectedProject.images"
                  :key="idx"
                  class="w-2 h-2 rounded-full transition-colors"
                  :class="idx === currentImageIndex ? 'bg-terminal-green' : 'bg-terminal-muted'"
                  @click="currentImageIndex = idx"
                ></button>
              </div>
            </div>
          </div>

          <!-- Modal Footer -->
          <div class="p-3 border-t border-terminal flex justify-between items-center">
            <span class="text-terminal-muted text-xs">
              {{ t.ui.pressEscArrows }}
            </span>
            <button class="command-btn text-xs notranslate" translate="no" @click="closeModal">
              [ {{ t.ui.close }} ]
            </button>
          </div>
        </div>
      </div>
    </Transition>

    <!-- Experience Modal -->
    <Transition name="fade">
      <div
        v-if="showExperienceModal && selectedExperience"
        class="fixed inset-0 z-50 flex items-center justify-center p-4"
        @click.self="closeExperienceModal"
      >
        <!-- Backdrop -->
        <div class="absolute inset-0 bg-black/80 backdrop-blur-sm"></div>

        <!-- Modal Content -->
        <div class="relative terminal-window w-full max-w-3xl max-h-[90vh] overflow-hidden">
          <!-- Modal Header -->
          <div class="terminal-header">
            <div class="traffic-lights">
              <button class="traffic-light traffic-light--red" @click="closeExperienceModal"></button>
              <span class="traffic-light traffic-light--yellow"></span>
              <span class="traffic-light traffic-light--green"></span>
            </div>
            <span class="text-terminal-muted text-sm flex-1 text-center">
              cat ./experience/{{ selectedExperience.id }}/README.md
            </span>
            <div class="w-14"></div>
          </div>

          <!-- Modal Body -->
          <div class="p-4 overflow-y-auto max-h-[calc(90vh-120px)]">
            <!-- Experience Header -->
            <div class="mb-4">
              <div class="flex items-center gap-2 flex-wrap">
                <h2 class="text-terminal-cyan text-lg font-bold">{{ selectedExperience.role }}</h2>
                <span v-if="selectedExperience.current" class="text-terminal-green text-xs px-2 py-0.5 bg-terminal-green/20 rounded">● {{ t.ui.current }}</span>
              </div>
              <div class="text-terminal-yellow text-base mt-1">{{ selectedExperience.company }}</div>
              <div class="text-terminal-muted text-sm mt-1">
                {{ selectedExperience.period }} · {{ selectedExperience.location }}
              </div>
            </div>

            <!-- Description -->
            <div class="mb-4">
              <div class="text-terminal-muted text-xs mb-2">$ cat description.txt</div>
              <p class="text-terminal-text text-sm pl-2 border-l-2 border-terminal">
                {{ selectedExperience.description }}
              </p>
            </div>

            <!-- Achievements -->
            <div class="mb-4">
              <div class="text-terminal-muted text-xs mb-2">$ cat achievements.log</div>
              <ul class="space-y-2">
                <li
                  v-for="(achievement, idx) in selectedExperience.achievements"
                  :key="idx"
                  class="flex items-start gap-2 text-sm"
                >
                  <span class="text-terminal-green">✓</span>
                  <span class="text-terminal-text">{{ achievement }}</span>
                </li>
              </ul>
            </div>

            <!-- Technologies -->
            <div>
              <div class="text-terminal-muted text-xs mb-2">$ ls ./technologies/</div>
              <div class="flex flex-wrap gap-2">
                <span
                  v-for="tech in selectedExperience.technologies"
                  :key="tech"
                  class="text-xs px-3 py-1 bg-terminal-blue/20 text-terminal-blue rounded border border-terminal-blue/30"
                >
                  {{ tech }}
                </span>
              </div>
            </div>
          </div>

          <!-- Modal Footer -->
          <div class="p-3 border-t border-terminal flex justify-between items-center">
            <span class="text-terminal-muted text-xs">
              {{ t.ui.pressEscToClose }}
            </span>
            <button class="command-btn text-xs notranslate" translate="no" @click="closeExperienceModal">
              [ {{ t.ui.close }} ]
            </button>
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
/* Transitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.scale-enter-active {
  transition: all 0.5s ease;
}

.scale-enter-from {
  opacity: 0;
  transform: scale(0.9);
}

.slide-up-enter-active {
  transition: all 0.6s ease-out;
}

.slide-up-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: var(--color-terminal-bg);
}

::-webkit-scrollbar-thumb {
  background: var(--color-terminal-border);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: var(--color-terminal-muted);
}

/* Button disabled state */
button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
