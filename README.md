# 202230124 이동교

## 9월 16일 (수)

# Next.js 학습 요약

## 1. 폴더 및 파일 컨벤션 (Folder & File Conventions)

Next.js(App Router 기준)는 파일 시스템 기반의 라우팅을 사용하며, 특수한 이름을 가진 파일들을 통해 레이아웃, 로딩, 에러 처리 등을 직관적으로 정의할 수 있습니다.

- **`page.js` / `page.tsx`**: 해당 경로(Route)의 고유한 UI를 담당하며, 퍼블릭(Public) 접근이 가능한 라우트를 생성합니다.
- **`layout.js` / `layout.tsx`**: 여러 페이지 간에 공유되는 공통 레이아웃(헤더, 푸터 등)을 정의합니다. 상태를 유지하며 페이지 전환 시 리렌더링되지 않습니다.
- **`template.js` / `template.tsx`**: `layout`과 유사하지만, 페이지가 이동할 때마다 **매번 새로운 인스턴스가 생성(마운트)**됩니다.
- **`loading.js` / `loading.tsx`**: React의 Suspense를 기반으로 작동하며, 해당 세그먼트의 데이터를 불러오는 동안 보여줄 로딩 UI(스켈레톤 등)를 정의합니다.
- **`error.js` / `error.tsx`**: 에러 발생 시 사용자에게 보여줄 Fallback UI를 정의합니다. 반드시 클라이언트 컴포넌트(`'use client'`)여야 합니다.
- **`not-found.js` / `not-found.tsx`**: 404 Not Found 상황에서 렌더링되는 컴포넌트입니다.

---

## 2. Next.js 동적 라우팅 (Dynamic Routes)

URL의 경로가 고정되지 않고 동적으로 변할 때 사용합니다.

- **기본 동적 라우트**: 폴더명을 대괄호로 감싸서 생성합니다.
  - 예: `app/blog/[id]/page.tsx` $\rightarrow$ `/blog/1`, `/blog/abc` 등으로 접속 가능
- **파라미터 전달 받기**: 컴포넌트의 `params` 프로퍼티를 통해 값을 비동기로(또는 동적으로) 꺼내어 사용할 수 있습니다.

  ```tsx
  // app/blog/[id]/page.tsx
  export default async function Page({
    params,
  }: {
    params: Promise<{ id: string }>;
  }) {
    const { id } = await params;
    return <div>My Post: {id}</div>;
  }
  ```

  # Next.js 프로젝트 구조화 (Organizing Your Project)

Next.js는 파일 시스템 기반의 라우팅 방식을 사용하므로, 프로젝트의 규모가 커질수록 체계적인 구조화 전략이 매우 중요합니다. Next.js 공식 문서에서 권장하는 대표적인 파일 및 폴더 구조화 방식은 다음과 같습니다.

## 1. 기능별 폴더 구조 (Feature-based / Colocation)

- **개념**: 관련된 컴포넌트, 스타일, 타입, 훅 등을 전역에 거대하게 두는 것이 아니라, **특정 기능이나 도메인(Feature) 단위로 응집**시켜 묶는 방식입니다.
- **특징**:
  - 코드를 찾기 쉽고 유지보수성이 향상됩니다.
  - 예: 전역에 `components/`, `hooks/`, `utils/`를 일일이 두기보다, 각 도메인 폴더 내부에 해당 기능 전용 파일을 응집시키는 형태입니다.

## 2. 프라이빗 폴더 (`_` 폴더)

- **개념**: 폴더명 앞에 언더스코어(`_`)를 붙여 생성하는 폴더입니다.
- **특징**:
  - Next.js 라우팅 시스템에서 **완전히 제외**됩니다. (예: `app/_components`는 URL 경로로 접근 불가)
  - 라우팅과 무관한 내부 컴포넌트, 유틸리티, 스타일 파일 등을 안전하게 숨기고 분리할 때 매우 유용합니다.

## 3. 라우트 그룹 (`(folder)`)

- **개념**: 폴더명을 소괄호(`(그룹명)`)로 감싸서 만드는 방식입니다.
- **특징**:
  - **URL 경로에 전혀 영향을 주지 않으면서** 폴더들을 논리적으로 그룹화할 수 있습니다.
  - 예: `app/(auth)/login/page.tsx`로 폴더를 구성하더라도, 실제 매핑되는 URL은 `/login`이 됩니다.
  - 레이아웃을 공유하지 않아야 하는 페이지들을 묶거나, 관리용 폴더로 구조를 깔끔하게 정돈할 때 사용합니다.

## 컴포넌트 구현 (components/SkeletonCard.tsx)

export default function SkeletonCard() {
return (

<div className="p-4 border rounded-md shadow animate-pulse space-y-4">
{/_ 이미지 영역 스켈레톤 _/}
<div className="w-full h-48 bg-gray-200 rounded-md"></div>

      {/* 텍스트 영역 스켈레톤 */}
      <div className="h-6 bg-gray-200 rounded w-3/4"></div>
      <div className="space-y-2">
        <div className="h-4 bg-gray-200 rounded"></div>
        <div className="h-4 bg-gray-200 rounded w-5/6"></div>
      </div>
    </div>

);
}

## loading.tsx에 적용

import SkeletonCard from "@/components/SkeletonCard";

export default function Loading() {
return (
<div className="grid grid-cols-3 gap-4 p-6">
<SkeletonCard/>
<SkeletonCard/>
<SkeletonCard/>
</div>
);
}

## 9월 9일 (수)

# Installation - 프로젝트 수동 생성

### [ package.json 파일에 스크립트 추가 ]

```json
// package.json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint",
    "lint:fix": "eslint --fix"
  }
}
```

- 등록한 스크립트는 애플리케이션 개발의 다양한 명령을 참조합니다.

- next dev: 개발 서버를 시작합니다.

- next build: 프로덕션을 위한 애플리케이션을 빌드합니다.

- next start: 프로덕션 서버를 시작합니다.

- next lint: ESLint를 실행합니다.

- 이제 Turbopack이 기본 번들러입니다. Webpack을 사용하려면 next dev --webpack 또는 next build --webpack을 실행하면 됩니다.

# Installation - 프로젝트 수동 생성

### [ app 디렉토리 생성 ]

- Next.js는 file-system Routing을 사용합니다. 즉, 애플리케이션의 Routing은 파일을 어떻게 구성되는가에 따라 결정됩니다.

- app 디렉토리를 생성하고, 그 안에 layout.tsx 파일을 생성합니다. 이 파일은 루트 레이아웃이 됩니다.

- 이 파일은 필수 파일이며 <html>과 <body> 태그를 포함해야 합니다.

```tsx
// app/layout.tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

# Installation - 프로젝트 수동 생성

- 초기 콘텐츠로 사용할 홈 페이지 "app/page.tsx"를 생성합니다.

```tsx
// app/page.tsx
export default function Page() {
  return <h1>Hello, Next.js!</h1>;
}
```

📁 app
├── layout.tsx ───┐
└── page.tsx ───┴──> /

# # 오류 처리

- 문서의 지시 대로만 처리하면 오류가 발생합니다.

- 타입스크립트 환경이 아니기 때문입니다.

- 타입스크립트(TypeScript) 환경에서 react와 react-dom을 사용할 수 있도록 타입 정의를 제공하는 패키지를 설치해야 합니다.

```bash
# Bash - pnpm
```

# Bash - npm

npm install -D @types/react @types/react-dom

| 구분          | 일반 설치 (`pnpm add <pkg>`)                    | 개발용 설치 (`pnpm add -D <pkg>`)                    |
| :------------ | :---------------------------------------------- | :--------------------------------------------------- |
| **등록 위치** | `package.json` 내 `dependencies`                | `package.json` 내 `devDependencies`                  |
| **용도**      | 실제 서비스 구동에 반드시 필요한 패키지         | 코드 빌드, 테스트, 린팅 등 개발할 때만 필요한 패키지 |
| **배포 환경** | 빌드 결과물에 포함되거나 프로덕션 서버에 설치됨 | `--production` 옵션 등으로 빌드/배포 시 제외됨       |
| **대표 예시** | React, Vue, Express, Axios, Lodash 등           | TypeScript, ESLint, Prettier, Vite, Jest 등          |

# # 이전 버전의 Next.js 프로젝트로 인식

# js나 jsx로 프로젝트를 진행하려고 해도 오류가 발생합니다.

# 이런 경우 추가로 react를 import 합니다.

### [ 수정 전 ]

```tsx
// app/layout.tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html>
      <body>{children}</body>
    </html>
  );
}
```

### [ 수정 후 ]

// app/layout.tsx
import React from 'react'

export default function RootLayout({
children,
}: {
children: React.ReactNode
}) {
return (

<html>
<body>{children}</body>
</html>
)
}

# Installation - 프로젝트 수동 생성

### [ 알아두면 좋은 정보 ]

- 루트 레이아웃을 만드는 것을 잊어버린 경우, Next.js는 개발 서버를 실행할 때(`next dev`) 자동으로 이 파일을 만듭니다.

- 프로젝트 루트에 있는 `src` 폴더를 사용하여 애플리케이션 코드와 구성 파일을 분리할 수 있습니다.

---

# Installation - 프로젝트 수동 생성

### [ public 디렉토리 생성 (선택 사항) ]

- 이미지, 글꼴 등의 정적 리소스를 저장하기 위한 public 디렉토리를 프로젝트 루트에 생성합니다.

- public 디렉토리를 생성하면 기본 URL(/)로 public 디렉토리 내부의 리소스를 참조할 수 있습니다.

- 예를 들어 `public/profile.png`는 `/profile.png`와 같이 참조할 수 있습니다.

```tsx
// app/page.tsx
import Image from "next/image";

export default function Page() {
  return (
    <Image alt="Profile" height="{100}" src="/profile.png" width="{100}" />
  );
}
```

# Installation - 프로젝트 수동 생성

### [ 개발 서버 실행 ]

1. 개발 서버를 시작하려면 다음 명령을 실행합니다: `pnpm dev`

2. 명령을 실행한 후 [http://localhost:3000](http://localhost:3000) 로 접속하면 App을 확인할 수 있습니다.

   > **# 수동으로 접속해야 합니다. (react와 다름)**

3. `app/page.tsx` 파일을 편집하고 저장하면, 브라우저를 통해 업데이트된 결과를 확인할 수 있습니다.

---

### [ TypeScript 설정 ] 최소 Typescript 버전 : v5.1.0

- Next.js는 TypeScript를 기본적으로 지원합니다.

- 프로젝트에 TypeScript를 추가하려면 파일 확장자를 `.ts` 또는 `.tsx`로 바꾸고 `next dev` 명령을 실행합니다.

  > **# 스크립트를 추가 했기 때문에 pnpm dev명령을 사용합니다.**

- 명령을 실행하면 Next.js가 필요한 종속성을 자동으로 설치하고, 권장 구성 옵션이 포함된 `tsconfig.json` 파일을 추가합니다.

---

# Installation - 프로젝트 수동 생성

### [ ESLint 설정 ]

- Next.js는 ESLint 또는 Biome을 사용한 린팅을 지원합니다.

- package.json의 scripts 파일을 통해 원하는 린터를 선택하고 직접 실행하세요.

```json
// package.json
{
  "scripts": {
    "lint": "eslint",
    "lint:fix": "eslint --fix"
  }
}
```

# Installation - 프로젝트 수동 생성

### [ ESLint 설정 ]

- Next.js는 ESLint 또는 Biome을 사용한 린팅을 지원합니다.

- package.json의 scripts 파일을 통해 원하는 린터를 선택하고 직접 실행하세요.

```json
// package.json
{
  "scripts": {
    "lint": "eslint",
    "lint:fix": "eslint --fix"
  }
}
```

# Installation - 프로젝트 수동 생성

- `baseUrl` 경로를 구성하는 것 외에도, "paths" 옵션을 사용하여 모듈 경로를 "별칭"로 사용할 수 있습니다.

- 예를 들어 다음 구성은 `@/components/*`를 `components/*`에 매핑합니다.

```json
// tsconfig.json or jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "src/",
    "paths": {
      "@/styles/*": ["styles/*"],
      "@/components/*": ["components/*"]
    }
  }
}
```

# # baseUrl의 근본적인 문제

# baseUrl은 TypeScript 6.0부터 더 이상 사용되지 않음(Deprecated) 되었고, 7.0에서 완전히 삭제되었습니다.

# 과거에는 절대 경로와 모듈 별칭(Path Aliases)을 설정하기 위해 baseUrl이 필수적이었으나, 현재는 번들러와 TypeScript 컴파일러가 발전하면서 불필요한 레거시 설정이 되었습니다.

### [ 과거 설정 방식 (삭제 대상) ]

```json
{
  "compilerOptions": {
    "baseUrl": ".", //삭제 대상
    "paths": {
      "@/components/*": ["components/*"]
    }
  }
}
```

```json
{
  "compilerOptions": {
    // 1️⃣ baseUrl 삭제
    "paths": {
      // 2️⃣ 상대 경로의 기준점(./)을 명시적으로 적어 줌
      "@/*": ["./src/*"],
      // 다음과 같은 별칭이 의미가 있을까?
      "@/components/*": ["./src/components/*"]
    }
  }
}
```

# Installation

### [ Next.js 앱 업그레이드 ]

- Next.js 버전을 최신 상태로 유지하는 것이 좋습니다.

- 각 릴리스에는 새로운 기능과 함께 보안 패치, 버그 수정 및 성능 최적화가 포함되어 있으므로 최신 버전을 유지하면 개별 업그레이드의 크기를 줄일 수 있습니다.

- 다음 upgrade명령을 사용합니다.

```bash
# pnpm
pnpm next upgrade

# npm
# npx @next/codemod upgrade

# yarn
# yarn next upgrade

# bun
# bunx @next/codemod upgrade
```

- next업그레이드를 진행하면 패키지 내에 포함된 문서도 함께 업데이트됩니다.

- 업그레이드를 진행하면 node_modules/next/dist/docs/ 경로에 있는 next 패키지 내의 문서도 업데이트됩니다.

# # 자동 생성되는 항목

# 강의에서는 프로젝트를 자동으로 생성해서 사용합니다.

# 다음은 프로젝트를 자동 생성할 때 자동으로 생성되는 항목입니다.

- package.json 파일에 scripts 자동 추가 / public 디렉토리

- TypeScript 사용(선택) : tsconfig.json 파일 생성

- Eslint 설정 (선택) : .eslintrc.json 대신 eslint.config.mjs 파일 생성

- Tailwind CSS 사용 (선택)

- src 디렉토리 사용 (선택)

- App Router(선택), app/layout.tsx 파일 및 app/page.tsx

- Turbopack 사용(선택)

- import alias 사용 (선택) : **No**로 해도 tsconfig.json에 "paths" 자동 생성.

\* 수동으로 프로젝트를 생성할 때 추가적으로 해야 하는 작업을 자동으로 처리해 줍니다.

---

# # 용어 정의

- 이 장부터 이후에 사용될 몇가지 용어에 대한 설명입니다.

- 원문에는 route라는 단어가 자주 등장하고, 사전적 의미로는 "경로"입니다.

- route(라우트)는 "경로"를 의미하고, routing(라우팅)은 "경로를 찾아가는 과정"을 의미합니다.

- 그런데 path도 "경로"로 번역하기 때문에의 구별을 위해 대부분 routing(라우팅)으로 번역했습니다.

- directory와 folder는 특별한 구분 없이 나옵니다.

- 최상위 폴더의 경우 directory로 하위 폴더는 folder로 쓰는 경우가 많지만 꼭 그렇지는 않습니다.

- directory와 folder는 OS에 따라 구분되는 용어이기 때문에 같은 의미로 이해하면 됩니다.

- segment는 routing과 관련이 있는 directory의 별칭 정도로 이해하면 됩니다.

---

## 9월 2일 (수)

Git의 핵심 구조 및 동작 원리

Git은 프로젝트 상태를 추적하기 위해 세 가지 주요 영역을 사용합니다:

1. **작업 디렉토리 (Working Directory)**: 실제로 코드를 작성하고 수정하는 로컬 디렉토리.
2. **스테이징 영역 (Staging Area / Index)**: 다음 커밋(스냅샷)에 포함할 변경 사항들을 모아두는 준비 공간.
3. **로컬 저장소 (Local Repository, `.git`)**: 커밋을 통해 확정된 영구적인 변경 이력(스냅샷)이 기록되는 공간.

```

```
