# 4.2. Rakendus: Elementide lisamine

### 1. Mine *GameElements.swift* faili ning kirjuta seal asuva *GameScene*'i sisse Eksmati kuvamiseks jägmine kood:

```swift
    func createEksmati() -> SKSpriteNode {
    

        eksmati = SKSpriteNode(imageNamed: "eksmati")
        eksmati.size = CGSize(width: 50, height: 50)
        eksmati.position = CGPoint(x:self.frame.midX, y:self.frame.midY)
        

        eksmati.physicsBody = SKPhysicsBody(circleOfRadius: eksmati.size.width / 2)
        eksmati.physicsBody?.linearDamping = 1.1
        eksmati.physicsBody?.restitution = 0
        

        eksmati.physicsBody?.categoryBitMask = CollisionBitMask.eksmatiCategory
        eksmati.physicsBody?.collisionBitMask = CollisionBitMask.postCategory | CollisionBitMask.groundCategory

        

        eksmati.physicsBody?.affectedByGravity = false
        eksmati.physicsBody?.isDynamic = true
        
        return eksmati
    }
```

### 2. Mine nüüd GameScene.swift kausta ning kirjuta *createScene* sisse, eelnevalt kirjutatud koodi taha järgnevad kaks rida:

   ```swift
        self.eksmati = createEksmati()
        self.addChild(eksmati)
   ```

___
**Vajuta nüüd play nuppu ning vaata simulatori abiga, kas Eksmati on ekraanil kuvatud**
___

**VIDEO(1.,2.):**

<a href="https://www.youtube.com/watch?v=_ImKsn8M7S8
" target="_blank"><img src="http://img.youtube.com/vi/_ImKsn8M7S8/0.jpg" 
alt="Projekti loomine" width="240" height="180" border="10" /></a>


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
        scoreLbl.position = CGPoint(x: self.frame.width / 2, y: self.frame.height / 2 + self.frame.height / 3.0)
        scoreLbl.text = "\(score)"
        scoreLbl.fontColor = UIColor(red: 0/255, green: 0/255, blue: 0/255, alpha: 1.0)
        scoreLbl.zPosition = 5
        scoreLbl.fontSize = 50
        scoreLbl.fontName = "HelveticaNeue-Bold"
        return scoreLbl
    }
    
    
    /* Loome parima skoori kuvamise ekraanil */
    func createHighscoreLabel() -> SKLabelNode {
        let highscoreLbl = SKLabelNode()
        highscoreLbl.position = CGPoint(x: self.frame.width - 100, y: self.frame.height - 45)
        if let highestScore = UserDefaults.standard.object(forKey: "highestScore"){
            highscoreLbl.text = "Highest EAP Score: \(highestScore)"
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

scoreLbl = createScoreLabel()
self.addChild(scoreLbl)
 
 
highscoreLbl = createHighscoreLabel()
self.addChild(highscoreLbl)

createLogo()
```
 
 
___
**Vajuta uuesti Xcode play nuppu ning vaata, kas ekraan kuvab kõik selle, mida ta kuvama peaks**
___ 

**VIDEO(3.,4.):**

<a href="https://youtu.be/w73dgJHEjp4
" target="_blank"><img src="http://img.youtube.com/vi/w73dgJHEjp4/0.jpg" 
alt="Projekti loomine" width="240" height="180" border="10" /></a>


# ÜLESANNE:
* Loo sarnaselt restardi nupule nüüd ise pausi nupu funktsioon järgmiste punktidega:
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
	* Teksti tüübiks(```fontName```) määra "HelveticaNeue"
	* ```return```
	* Nüüd mine GameScene.swift faili ning kutsu sarnaselt skoori ja parima skoori kuvamisele välja ka taptoplayLbl 

>Kindlasti kontrolli simulatoris, kas loodud funktsioonid kuvatakse ekraanil nii nagu peab.

### Teadmiste kontroll:

* Kontrolli oma teadmisi [LearningApps](https://learningapps.org/watch?v=p3j5n93dc18) lehel.

___


