# SonicWall Cloud Secure Edge（CSE）によるリモートアクセス
文書更新日:2025-03-20

## 設定ガイドの目的

本ガイドは、SonicWallのCloud Secure Edge、SingleID、及びSynology NASを統合したソリューションを用いて、マイクロSMB向けにセキュアかつ高速で、利便性とコスト効率に優れたリモートアクセス環境を構築するための手順とベストプラクティスを提供することを目的としています。具体的には、以下の観点でメリットを実現します。

- **認証の強化**  
  SingleIDによるシングルサインオン（SSO）とCloud Secure Edgeの証明書認証を組み合わせることで、ユーザーとデバイス双方の認証レベルが大幅に向上します。これにより、従来のVPN接続よりも高いセキュリティを確保し、認証管理の手間を軽減します。

- **高速なアクセスと通信の最適化**  
  Cloud Secure Edgeは、WireGuardを活用して暗号化トンネルを構築し、リモートアクセス時の接続速度と通信遅延を最適化します。WireGuardのシンプルな設計と効率的な暗号化処理により、SSL VPNやIPSec VPNよりも高速な通信が実現されます。また、Synology NASを経由することで、さらに効率的なコネクションが確立され、業務効率とユーザー体験が向上します。

- **コスト効率の向上**  
  従来、専用のコネクターをオンプレミスに設置する場合と比較して、Synology NASをコネクターとして活用することで、大幅なコスト削減が実現できます。シンプルな構成とクラウドサービスの活用により、初期導入費用および運用コストが最適化され、全体のコストパフォーマンスが向上します。

- **利便性の向上**  
  Cloud Secure Edgeを利用することで、インターネット経由でのアクセスでも、あたかも社内にいるかのような使い勝手と一貫したユーザーエクスペリエンスが実現され、管理者およびエンドユーザー双方に大きな利便性向上をもたらします。

このガイドは、システム管理者やセキュリティ担当者が、これらのメリットを最大限に活かし、迅速かつ確実にセキュアで効率的なアクセス環境を構築・運用できるよう、具体的な設定手順、注意点、およびトラブルシューティングのポイントを詳細にまとめたものです。

## 設定手順
1. [**SAMLでSingleIDと連携する手順**](../sonicwall_cse.md){target=_blank}
2. [**CSE Connectorを設定する手順**](./configure_cse_connector.md){target=_blank}
3. [**Synology NASに仮想マシンを構築する手順**](./create_alpine_vm_synology.md){target=_blank}
4. [**CSE Connectorをインストールする手順**](./install_cse_connector_alpine.md){target=_blank}
5. [**Service Tunnelを設定する手順**](./configure_service_tunnel.md){target=_blank}


