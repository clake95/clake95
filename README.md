config user saml
    edit "azure"
        set cert "Fortinet_Factory"
        set entity-id "https://50.175.82.150/remote/saml/metadata"
        set single-sign-on-url "https://50.175.82.150/remote/saml/login"
        set single-logout-url "https://50.175.82.150/remote/saml/logout"
        set idp-entity-id "https://sts.windows.net/43795e9c-df0d-467a-a8bf-45d9838e9c3d/"
        set idp-single-sign-on-url "https://login.microsoftonline.com/43795e9c-df0d-467a-a8bf-45d9838e9c3d/saml2"
        set idp-single-logout-url "https://login.microsoftonline.com/43795e9c-df0d-467a-a8bf-45d9838e9c3d/saml2"
        set idp-cert "REMOTE_Cert_1"
        set user-name "username"
        set group-name "group"
        set digest-method sha256
    next
end
config user group
    edit "SSO_Guest_Users"
    next
    edit "Guest-group"
        set member "guest"
    next
    edit "FortiGateAccess"
        set member "azure"
        config match
            edit 1
                set server-name "azure"
                set group-name "32a4d618-51a7-456d-959e-3df655ba3a71"
            next
        end
    next
end
config vpn ssl web portal
    edit "full-access"
        set tunnel-mode enable
        set ipv6-tunnel-mode enable
        set web-mode enable
        set ip-pools "SSLVPN_TUNNEL_ADDR1"
        set split-tunneling disable
        set ipv6-pools "SSLVPN_TUNNEL_IPv6_ADDR1"
        config bookmark-group
            edit "gui-bookmarks"
            next
        end
    next
    edit "web-access"
        set web-mode enable
    next
    edit "tunnel-access"
        set tunnel-mode enable
        set ipv6-tunnel-mode enable
        set ip-pools "SSLVPN_TUNNEL_ADDR1"
        set ipv6-pools "SSLVPN_TUNNEL_IPv6_ADDR1"
    next
    edit "No Access"
        set forticlient-download disable
    next
end
config vpn ssl settings
    set servercert "Fortinet_Factory"
    set idle-timeout 28800
    set tunnel-ip-pools "SSLVPN_TUNNEL_ADDR1"
    set tunnel-ipv6-pools "SSLVPN_TUNNEL_IPv6_ADDR1"
    set port 443
    set source-interface "wan1"
    set source-address "all"
    set source-address6 "all"
    set default-portal "No Access"
    config authentication-rule
        edit 1
            set groups "FortiGateAccess"
            set portal "full-access"
        next
    end
end
config system global
    set remoteauthtimeout 240
end
