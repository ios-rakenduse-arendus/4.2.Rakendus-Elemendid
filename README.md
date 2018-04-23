# 4.2. Rakendus: Elementide lisamine

### 1. Mine *GameElements.swift* faili ning kirjuta seal asuva *GameScene*'i sisse Eksmati kuvamiseks jägmine kood:

```swift
    func createEksmati() -> SKSpriteNode {
    
       /* Loome Eksmati ning paneme paika ta mõõdud ja asukoha */
        eksmati = SKSpriteNode(imageNamed: "eksmati")
        eksmati.size = CGSize(width: 50, height: 50)
        eksmati.position = CGPoint(x:self.frame.midX, y:self.frame.midY)
        
       /* Teeme Eksmatist pallile sarnaneva füüsilise keha (SKPhysicsBody) */
        eksmati.physicsBody = SKPhysicsBody(circleOfRadius: eksmati.size.width / 2)
        eksmati.physicsBody?.linearDamping = 1.1
        eksmati.physicsBody?.restitution = 0
        
       /* Paneme Eksmati füüsilise keha reageerima teiste kehadega */
        eksmati.physicsBody?.categoryBitMask = CollisionBitMask.eksmatiCategory
        eksmati.physicsBody?.collisionBitMask = CollisionBitMask.postCategory | CollisionBitMask.groundCategory

        
       /* Paneme Eksmati reageerima gravitatsioonile */
        eksmati.physicsBody?.affectedByGravity = false
        eksmati.physicsBody?.isDynamic = true
        
        return eksmati
    }
```

### 2. Mine nüüd GameScene.swift kausta ning kirjuta *createScene* sisse, eelnevalt kirjutatud koodi taha järgnevad kaks rida:

   ```swift
   
   /* Kutsume GameElements.swift failis loodud Eksmati välja */ 
        self.eksmati = createEksmati()
        self.addChild(eksmati)
   ```

___
**Vajuta nüüd play nuppu ning vaata simulatori abiga, kas Eksmati on ekraanil kuvatud**
___

# ÜLESANNE: 
* Pane Eksmati füüsilise keha (*physicsBody*) koos *contactTestBitMask*'iga reageerima posti kategooria kokkupõrkamisega, EAP kategooria kokkupõrkamisega ning maapinna (*ground*) kategooria kokkupõrkamisega.
>**Vaata näiteks eelnevat koodiriba, kus me määrasime Eksmati füüsilise keha koos *collisionBitMask*'iga reageerima posti ja maapinna (ground) kategooriaga ning imiteeri**
* Muuda Eksmati kõrguselt ja laiuselt näiteks kümne palli võrra suuremaks ning vaata simulatoriga, mis juhtub.
___

### 3. Läheme tagasi *GameElements.swift* faili ning lisame Eksmati funktsiooni alla ka teised, meile vajalikud mänguelementide funktsioonid (va. postid ja EAP'd):


```swift

/* Loome restart nupu */
func createRestartBtn() {
        restartBtn = SKSpriteNode(imageNamed: "restart")
        restartBtn.size = CGSize(width:100, height:100)
        restartBtn.position = CGPoint(x: self.frame.width / 2, y: self.frame.height / 2)
        restartBtn.zPosition = 6
        restartBtn.setScale(0)
        self.addChild(restartBtn)
        restartBtn.run(SKAction.scale(to: 1.0, duration: 0.3))
    }
    
   
   
   /* Loome skoori kuvamise ekraanil */
    func createScoreLabel() -> SKLabelNode {
        let scoreLbl = SKLabelNode()
        scoreLbl.position = CGPoint(x: self.frame.width / 2, y: self.frame.height / 2 + self.frame.height / 2.6)
        scoreLbl.text = "\(score)"
        scoreLbl.zPosition = 5
        scoreLbl.fontSize = 50
        scoreLbl.fontName = "HelveticaNeue-Bold"
        
        let scoreBg = SKShapeNode()
        scoreBg.position = CGPoint(x: 0, y: 0)
        scoreBg.path = CGPath(roundedRect: CGRect(x: CGFloat(-50), y: CGFloat(-30), width: CGFloat(100), height: CGFloat(100)), cornerWidth: 50, cornerHeight: 50, transform: nil)
        let scoreBgColor = UIColor(red: CGFloat(0.0), green: CGFloat(0.0), blue: CGFloat(0.0), alpha: CGFloat(1.0))
        scoreBg.strokeColor = UIColor.clear
        scoreBg.fillColor = scoreBgColor
        scoreBg.zPosition = -1
        scoreLbl.addChild(scoreBg)
        return scoreLbl
    }
    
    
    /* Loome parima skoori kuvamise ekraanil */
    func createHighscoreLabel() -> SKLabelNode {
        let highscoreLbl = SKLabelNode()
        highscoreLbl.position = CGPoint(x: self.frame.width - 80, y: self.frame.height - 22)
        if let highestScore = UserDefaults.standard.object(forKey: "highestScore"){
            highscoreLbl.text = "Highest Score: \(highestScore)"
        } else {
            highscoreLbl.text = "Highest Score: 0"
        }
        highscoreLbl.fontColor = UIColor(red: CGFloat(0.0), green: CGFloat(0.0), blue: CGFloat(0.0), alpha: CGFloat(1.0))
        highscoreLbl.zPosition = 5
        highscoreLbl.fontSize = 15
        highscoreLbl.fontName = "Helvetica-Bold"
        return highscoreLbl
    }
    
    /* Loome logo kuvamise ekraanil */
    func createLogo() {
        logoImg = SKSpriteNode()
        logoImg = SKSpriteNode(imageNamed: "logo")
        logoImg.size = CGSize(width: 272, height: 65)
        logoImg.position = CGPoint(x:self.frame.midX, y:self.frame.midY + 100)
        logoImg.setScale(0.5)
        self.addChild(logoImg)
        logoImg.run(SKAction.scale(to: 1.0, duration: 0.3))
    }
```

### 4. Lähme uuesti tagasi *GameScene.swift* faili ning kutsume välja eelmises punktis loodud elementide funktsioonid (neil millel vaja) samamoodi ja samasse kohta nagu tegime Eksmatiga:
 

```swift
/* Kutsume GameElements.swift failis loodud skoori välja */ 
scoreLbl = createScoreLabel()
self.addChild(scoreLbl)
 
 
/* Kutsume GameElements.swift failis loodud parima skoori välja */ 
highscoreLbl = createHighscoreLabel()
self.addChild(highscoreLbl)


/* Kutsume GameElements.swift failis loodud logo välja */  
createLogo()
```
 
 
___
**Vajuta uuesti Xcode play nuppu ning vaata, kas ekraan kuvab kõik selle, mida ta kuvama peaks**

___ 
# ÜLESANNE:
* Loo sarnaselt restardi nupule nüüd ise pausi nupu funktsioon järgmiste puntidega:
    * Määra nupu kuvamiseks õige pildi nimi
    * Määra pausi nupu laiuseks ja kõrguseks 40
    * Määra pausi nupu positsiooni **x** kordinaadiks *self.frame.width - 30* ning **y** kordinaadiks *30*
    * Määra pausi nupu *zPosition* sama, mis restart nupul
    * *addChild*
* Loo sarnaselt skoori kuvamise algusele *taptoplayLbl* tekst järgmiste punktidega:
	* Pane *taptoplayLbl* võrduma teksti kuvamise node'iga
	* Positsiooni **x** kordinaadiks määra *self.frame.midX* ning **y** kordinaadiks määra *self.frame.midY - 100*
	* Määra tekst, mida kuvada. Näiteks ***"Tap anywhere to play"*** või ***"Mängimiseks puuduta ekraani"***
	>Kui paned siin teksti eestikeelseks siis viisakusest muuda ka parima skoori funktsioonis kuvatav tekst eestikeelseks.
	>
	* Määra teksti värviks(```fontColor```) must ehk pane kõik peamised värvid nulliks ning alpka väärtuseks 1.0
	* ```zPosition``` pane võrduma 5'ga
	* Teksti suuruseks(```fontSize```) määra 20
	* Teksti tüübiks(```fontName``` määra "HelveticaNeue"
	* ```return```
	* Nüüd mine GameScene.swift faili ning kutsu sarnaselt skoori ja parima skoori kuvamisele välja ka taptoplayLbl 

>Kindlasti kontrolli simulatoris, kas loodud funktsioonid kuvatakse ekraanil nii nagu peab.
___


