# xray-hijk-fix.sh（不推荐）

### 修复https://github.com/hijkpw/scripts/blob/master/xray.sh 选择8执行报错

##### 修复案例（仅供学习）
```
报错说明：（域名未解析到当前服务器IP）

参考：https://github.com/linabellbiu/xray

尝试修复：（查看脚本源码得知）

if [[ -f ~/xray.pem && -f ~/xray.key ]]; then
    colorEcho ${BLUE}  " 检测到自有证书，将使用其部署"
    CERT_FILE="/usr/local/etc/xray/${DOMAIN}.pem"
    KEY_FILE="/usr/local/etc/xray/${DOMAIN}.key"
else
    resolve=`curl -sL https://hijk.art/hostip.php?d=${DOMAIN}`
    res=`echo -n ${resolve} | grep ${IP}`
    if [[ -z "${res}" ]]; then
        colorEcho ${BLUE}  "${DOMAIN} 解析结果：${resolve}"
        colorEcho ${RED}  " 域名未解析到当前服务器IP(${IP})!"
        exit 1
    fi
fi

找到原因：https://hijk.art 无法生成证书xray.pem和xray.key

证书问题：手上只有xray.crt与xray.key，通过crt证书转换为pem即可，放入目录/root和/usr/local/etc/xray即可。
参考：https://www.volcengine.com/theme/1119955-S-7-1
sudo apt-get install openssl
openssl x509 -in certificate.crt -out certificate.pem -outform PEM
```