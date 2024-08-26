# VPC
以下に、従来のVPCサービスとAmazon VPC Latticeの違いをアーキテクチャ図で示します。Mermaidを使用して、各アプローチの構造を視覚的に説明します。

### 1. **従来のVPCサービス（VPCピアリング + Transit Gateway）**

```mermaid
graph TD
    subgraph "VPC A (アカウント1)"
        A1[アプリケーション1]
        A2[アプリケーション2]
    end

    subgraph "VPC B (アカウント2)"
        B1[アプリケーション3]
        B2[アプリケーション4]
    end

    subgraph "VPC C (アカウント3)"
        C1[アプリケーション5]
        C2[アプリケーション6]
    end

    A1 -- ピアリング接続 --> B1
    B1 -- ピアリング接続 --> C1
    A2 -- ピアリング接続 --> B2
    B2 -- ピアリング接続 --> C2

    A1 -. Transit Gateway .-> B1
    A2 -. Transit Gateway .-> B2
    B1 -. Transit Gateway .-> C1
    B2 -. Transit Gateway .-> C2

```

### 2. **Amazon VPC Lattice**

```mermaid
graph TD
    subgraph "VPC A (アカウント1)"
        A1[アプリケーション1]
        A2[アプリケーション2]
    end

    subgraph "VPC B (アカウント2)"
        B1[アプリケーション3]
        B2[アプリケーション4]
    end

    subgraph "VPC C (アカウント3)"
        C1[アプリケーション5]
        C2[アプリケーション6]
    end

    subgraph "VPC Lattice"
        L1[サービスネットワーク]
        L2[サービスディスカバリー]
    end

    A1 -- サービスネットワーク --> L1
    A2 -- サービスネットワーク --> L1
    B1 -- サービスネットワーク --> L1
    B2 -- サービスネットワーク --> L1
    C1 -- サービスネットワーク --> L1
    C2 -- サービスネットワーク --> L1

    L1 --> L2

    
    class L1,L2 lattice;
```

### 3. **図解の説明**

#### **従来のVPCサービス（VPCピアリング + Transit Gateway）**
- **VPCピアリング**:
  - VPC間を1対1で接続します。VPCが増えると接続が複雑になります。
  - 各アプリケーション間の通信が直接接続されるため、管理が煩雑になります。

- **Transit Gateway**:
  - 複数のVPCを集中的に接続し、ルーティングを統合します。
  - 各VPCのアプリケーションはTransit Gatewayを経由して通信できますが、サービスディスカバリーは手動で設定する必要があります。

#### **Amazon VPC Lattice**
- **VPC Lattice**:
  - 各VPC内のアプリケーションをサービスネットワークに統合し、VPC Latticeを介して通信を一元管理します。
  - **サービスディスカバリー**が組み込まれており、アプリケーション間の通信が自動的に管理されます。
  - マルチアカウントやマルチVPCにまたがるマイクロサービスアーキテクチャを簡素化します。

### 4. **まとめ**

- **従来のVPCサービス**では、複数のVPC間の通信が複雑で、サービスディスカバリーも手動で設定する必要があります。
- **Amazon VPC Lattice**は、通信管理とサービスディスカバリーを一元化し、マルチアカウント・マルチVPCの環境でも簡素に管理できます。

このアーキテクチャ図を通じて、従来のVPCサービスとAmazon VPC Latticeの違いを視覚的に理解することができます。
