# 배포 인덱스

[Nebula](https://github.com/dscalzi/Nebula)를 사용하여 배포 인덱스 생성을 자동화할 수 있습니다.

배포 사양의 최신이자 정확한 설명은 [helios-distribution-types](https://github.com/dscalzi/helios-distribution-types)에서 확인할 수 있습니다.

배포 인덱스는 JSON 형식으로 작성됩니다. 인덱스의 일반 형태는 아래와 같습니다.

```json
{
    "version": "1.0.0",
    "discord": {
        "clientId": "12334567890123456789",
        "smallImageText": "WesterosCraft",
        "smallImageKey": "seal-circle"
    },
    "rss": "https://westeroscraft.com/articles/index.rss",
    "servers": [
        {
            "id": "Example_Server",
            "name": "WesterosCraft Example Client",
            "description": "Example WesterosCraft server. Connect for fun!",
            "icon": "http://mc.westeroscraft.com/WesterosCraftLauncher/files/example_icon.png",
            "version": "0.0.1",
            "address": "mc.westeroscraft.com:1337",
            "minecraftVersion": "1.11.2",
            "discord": {
                "shortId": "Example",
                "largeImageText": "WesterosCraft Example Server",
                "largeImageKey": "server-example"
            },
            "mainServer": true,
            "autoconnect": true,
            "modules": [
                "Module Objects Here"
            ]
        }
    ]
}
```

## 배포 인덱스 객체

#### 예시
```json
{
    "version": "1.0.0",
    "discord": {
        "clientId": "12334567890123456789",
        "smallImageText": "WesterosCraft",
        "smallImageKey": "seal-circle"
    },
    "rss": "https://westeroscraft.com/articles/index.rss",
    "servers": []
}
```

### `DistroIndex.version: string/semver`

인덱스 형식의 버전입니다. 향후 업데이트를 순차적으로 적용할 때 사용됩니다.

### `DistroIndex.discord: object`

[Discord Rich Presence](https://discordapp.com/developers/docs/rich-presence/how-to)에 사용할 전역 설정입니다.

**속성**

- `discord.clientId: string`  
  Discord에 등록된 애플리케이션의 클라이언트 ID입니다.
- `discord.smallImageText: string`  
  작은 아이콘(`smallImageKey`)에 마우스를 올렸을 때 표시될 툴팁 텍스트입니다.
- `discord.smallImageKey: string`  
  작은 프로필 아트워크로 업로드된 이미지의 이름입니다.

### `DistroIndex.rss: string/url`

뉴스 로드에 사용할 RSS 피드의 URL입니다.

---

## 서버 객체

#### 예시
```json
{
    "id": "Example_Server",
    "name": "WesterosCraft Example Client",
    "description": "Example WesterosCraft server. Connect for fun!",
    "icon": "http://mc.westeroscraft.com/WesterosCraftLauncher/files/example_icon.png",
    "version": "0.0.1",
    "address": "mc.westeroscraft.com:1337",
    "minecraftVersion": "1.11.2",
    "discord": {
        "shortId": "Example",
        "largeImageText": "WesterosCraft Example Server",
        "largeImageKey": "server-example"
    },
    "mainServer": true,
    "autoconnect": true,
    "modules": []
}
```

### `Server.id: string`

서버의 고유 ID입니다. 이 ID로 런처는 모드 설정 및 선택된 서버 정보를 저장합니다. ID가 변경되면 이전 ID에 연관된 모든 데이터가 삭제됩니다.

### `Server.name: string`

유저 인터페이스(UI)에 표시될 서버 이름입니다.

### `Server.description: string`

UI에 표시되는 간략한 서버 설명입니다.

### `Server.icon: string/url`

서버 아이콘의 URL입니다. UI에 표시됩니다.

### `Server.version: string/semver`

서버 구성 파일의 버전입니다.

### `Server.address: string/url`

서버의 IP 주소(및 포트)입니다.

### `Server.minecraftVersion: string`

서버가 실행 중인 Minecraft 버전입니다.

### `Server.discord: object`

서버별 Discord Rich Presence 설정입니다.

**속성**

- `discord.shortId: string`  
  두 번째 상태 줄에 `Server: shortId` 형태로 표시될 짧은 서버 ID입니다.
- `discord.largeImageText: string`  
  큰 아이콘(`largeImageKey`)의 툴팁 텍스트입니다.
- `discord.largeImageKey: string`  
  큰 프로필 아트워크로 업로드된 이미지의 이름입니다.

### `Server.mainServer: boolean`

배열 내에서 단 하나의 서버만 `mainServer`를 `true`로 설정해야 합니다.  
- 이전에 선택된 서버가 유효하지 않거나 선택된 서버가 없을 때, 기본으로 자동 선택됩니다.  
- 어느 서버도 설정하지 않으면 배열의 첫 번째 서버가 기본으로 선택됩니다.  
- 여러 서버에 `mainServer:true`가 설정되면, 런처가 먼저 발견한 서버가 유효합니다.  
- 기본 서버가 아닌 경우 `false` 대신 속성을 아예 생략할 수 있습니다.

### `Server.autoconnect: boolean`

사용자가 자동 연결 설정을 켜더라도 이 서버가 자동 연결 가능한지 여부를 지정합니다.  
- `false`인 경우 자동 연결 대상에서 제외됩니다.

### `Server.javaOptions: JavaOptions`

> **선택 사항**  
> 서버별 Java 실행 옵션입니다. 제공하지 않으면 클라이언트 기본값을 사용합니다.

### `Server.modules: Module[]`

모듈 객체 배열입니다.

---

## JavaOptions 객체

서버별 Java 옵션을 설정합니다.

#### 예시
```json
{
  "supported": ">=17",
  "suggestedMajor": 17,
  "platformOptions": [
    {
      "platform": "darwin",
      "architecture": "arm64",
      "distribution": "CORRETTO"
    }
  ],
  "ram": {
      "recommended": 3072,
      "minimum": 2048
  }
}
```

### `JavaOptions.platformOptions: JavaPlatformOptions[]`

> **선택 사항**  
> 플랫폼별 Java 검증 규칙을 정의합니다. 정의되지 않은 속성은 클라이언트 내장 로직으로 처리됩니다.  
> 우선순위 (높→낮):  
> 1. 현재 플랫폼 + 현재 아키텍처  
> 2. 현재 플랫폼 + 모든 아키텍처  
> 3. JavaOptions 기본 속성  
> 4. 클라이언트 기본 로직

**속성**  
- `platform`: 규칙 적용 플랫폼  
- `architecture`: (선택) 적용 아키텍처. 없으면 모든 아키텍처  
- `distribution`: (선택) JDK 배포판  
- `supported`: (선택) 지원하는 버전 범위(semver)  
- `suggestedMajor`: (선택) 권장 메이저 버전

### `JavaOptions.ram: object`

> **선택 사항**  
> 서버 인스턴스별 최소·권장 RAM(MB)을 지정합니다. 512MB 단위여야 합니다.  
> - `ram.recommended`: 권장 RAM (MB)  
> - `ram.minimum`: 최소 RAM (MB)

### `JavaOptions.distribution: string`

> **선택 사항**  
> 찾은 JDK가 없을 때 다운로드할 기본 배포판입니다. 지정 없으면 클라이언트가 결정합니다.

### `JavaOptions.supported: string`

> **선택 사항**  
> 지원하는 JDK 버전 범위(semver)입니다.

### `JavaOptions.suggestedMajor: number`

> **선택 사항**  
> 권장 메이저 Java 버전입니다. `supported`를 지정하면 필수 설정해야 합니다.
