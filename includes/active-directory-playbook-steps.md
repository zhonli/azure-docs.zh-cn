## <a name="playbook-steps"></a>演练手册步骤
1. [介绍](../articles/active-directory/active-directory-playbook-intro.md)
2. [成分](../articles/active-directory/active-directory-playbook-ingredients.md)
    * [主题](../articles/active-directory/active-directory-playbook-ingredients.md)
    * [环境](../articles/active-directory/active-directory-playbook-ingredients.md#theme)
    * [目标用户](../articles/active-directory/active-directory-playbook-ingredients.md#environment)
3. [实现](../articles/active-directory/active-directory-playbook-implementation.md)
   * [Foundation - 将 AD 同步到 Azure AD](../articles/active-directory/active-directory-playbook-implementation.md#foundation---syncing-ad-to-azure-ad)
     * [将本地标识扩展到云](../articles/active-directory/active-directory-playbook-implementation.md#extending-your-on-premises-identity-to-the-cloud)  
     * [使用组分配 Azure AD 许可证](../articles/active-directory/active-directory-playbook-implementation.md#assigning-azure-ad-licenses-using-groups)
   * [主题 - 多个应用，一个标识](../articles/active-directory/active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity)
     * [集成 SaaS 应用程序 - 联合 SSO](../articles/active-directory/active-directory-playbook-implementation.md#integrate-saas-applications---federated-sso)
     * [SSO 和标识生命周期事件](../articles/active-directory/active-directory-playbook-implementation.md#sso-and-identity-lifecycle-events)
     * [集成 SaaS 应用程序 - 密码 SSO](../articles/active-directory/active-directory-playbook-implementation.md#integrate-saas-applications---password-sso)
     * [对共享帐户进行安全访问](../articles/active-directory/active-directory-playbook-implementation.md#secure-access-to-shared-accounts)
     * [对本地应用程序进行安全的远程访问](../articles/active-directory/active-directory-playbook-implementation.md#secure-remote-access-to-on-premises-applications)
     * [将 LDAP 标识同步到 Azure AD](../articles/active-directory/active-directory-playbook-implementation.md#synchronize-ldap-identities-to-azure-ad)
   * [主题 - 增强安全性](../articles/active-directory/active-directory-playbook-implementation.md#theme---increase-your-security)
     * [确保管理员帐户访问安全](../articles/active-directory/active-directory-playbook-implementation.md#secure-administrator-account-access)
     * [确保应用程序访问安全](../articles/active-directory/active-directory-playbook-implementation.md#secure-access-to-applications)
     * [启用实时 (JIT) 管理](../articles/active-directory/active-directory-playbook-implementation.md#enable-just-in-time-jit-administration)
     * [根据风险来保护标识](../articles/active-directory/active-directory-playbook-implementation.md#protect-identities-based-on-risk)
     * [使用基于证书的身份验证，在没有密码的情况下进行身份验证](../articles/active-directory/active-directory-playbook-implementation.md#authenticate-without-passwords-using-certificate-based-authentication)
   * [主题 - 通过自助服务进行缩放](../articles/active-directory/active-directory-playbook-implementation.md#theme---scale-with-self-service)
     * [自助密码重置](../articles/active-directory/active-directory-playbook-implementation.md#self-service-password-reset)
     * [对应用程序进行自助访问](../articles/active-directory/active-directory-playbook-implementation.md#self-service-access-to-applications)
4. [构建基块](../articles/active-directory/active-directory-playbook-building-blocks.md)
   * [执行组件目录](../articles/active-directory/active-directory-playbook-building-blocks.md#catalog-of-actors)
   * [所有构建基块的常见先决条件](../articles/active-directory/active-directory-playbook-building-blocks.md#common-prerequisites-for-all-building-blocks)
   * [目录同步 - 密码哈希同步 (PHS) - 新安装](../articles/active-directory/active-directory-playbook-building-blocks.md#directory-synchronization---password-hash-sync-phs---new-installation)
   * [品牌](../articles/active-directory/active-directory-playbook-building-blocks.md#branding)
   * [基于组的许可](../articles/active-directory/active-directory-playbook-building-blocks.md#group-based-licensing)
   * [SaaS 联合 SSO 配置](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-federated-sso-configuration)
   * [SaaS 密码 SSO 配置](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-password-sso-configuration)
   * [SaaS 共享帐户配置](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-shared-accounts-configuration)
   * [应用代理配置](../articles/active-directory/active-directory-playbook-building-blocks.md#app-proxy-configuration)
   * [通用 LDAP 连接器配置](../articles/active-directory/active-directory-playbook-building-blocks.md#generic-ldap-connector-configuration)
   * [组 - 委派的所有权](../articles/active-directory/active-directory-playbook-building-blocks.md#groups---delegated-ownership)
   * [SaaS 和标识生命周期](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-and-identity-lifecycle)
   * [对应用程序管理进行自助访问](../articles/active-directory/active-directory-playbook-building-blocks.md#self-service-access-to-application-management)
   * [自助密码重置](../articles/active-directory/active-directory-playbook-building-blocks.md#self-service-password-reset)
   * [通过手机通话进行 Azure 多重身份验证](../articles/active-directory/active-directory-playbook-building-blocks.md#azure-multi-factor-authentication-with-phone-calls)
   * [SaaS 应用程序的 MFA 条件性访问](../articles/active-directory/active-directory-playbook-building-blocks.md#mfa-conditional-access-for-saas-applications)
   * [Privileged Identity Management (PIM)](../articles/active-directory/active-directory-playbook-building-blocks.md#privileged-identity-management-pim)
   * [发现风险事件](../articles/active-directory/active-directory-playbook-building-blocks.md#discovering-risk-events)
   * [部署登录风险策略](../articles/active-directory/active-directory-playbook-building-blocks.md#deploying-sign-in-risk-policies)
   * [配置基于证书的身份验证](../articles/active-directory/active-directory-playbook-building-blocks.md#configuring-certificate-based-authentication)