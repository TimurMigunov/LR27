# LR27

Мигунов Т.У.

ЭВТ-70

Игровой Движок: Unity.

Лабораторная работа №27

Тема: Разработка игрового проекта пинг-понг

Цель: приобрести навыки в разработке игрового проекта пинг-понг

Ход работы:

1.	Выполнение работы

1.	Создание папок Fonts, Materials, Scenes, Scripts, Sprites.

_____________________________![image](https://user-images.githubusercontent.com/119228138/205036707-e9770827-eba0-4529-a2bf-cd2b45609327.png)

Рис. 27.1. Создание папок

2.	Параметр Bouncy
____________________________![image](https://user-images.githubusercontent.com/119228138/205036838-a57f25f7-e6b6-4d7f-9e93-5ecf616ca94f.png)

Рис.27.2  Параметры Bouncy

____________________________![image](https://user-images.githubusercontent.com/119228138/205037055-74b47f43-523d-4ddf-bea4-f70013bb304a.png)


 
_____________________________Рис.27.3 – Параметры Bouncy


3.	Параметр Player paddle
 
_____________________________![image](https://user-images.githubusercontent.com/119228138/205037581-731000d2-8935-4fb2-9484-42df7dcc616a.png)
 
_____________________________Рис. 27.4. Настройка Player paddle

4.	Параметр Computer paddle 

_____________________________![image](https://user-images.githubusercontent.com/119228138/205037564-d7dd89cb-63e4-4b57-9c06-13a5bf7c27f4.png)

_____________________________Рис. 27.5. Настройка Computer paddle

5.	Параметр Top Wall
 
_____________________________![image](https://user-images.githubusercontent.com/119228138/205037534-e0f1c613-6510-40bf-b381-6bdde44fc836.png)


_____________________________Рис. 27.6. Настройка Top Wall

6.	Параметр Bottom Wall

_____________________________![image](https://user-images.githubusercontent.com/119228138/205037480-757bcedb-1830-4759-9135-607fe79d0628.png)

_____________________________Рис. 27.7. Настройка Bottom Wall

7.	Параметр Left Wall
 
_____________________________![image](https://user-images.githubusercontent.com/119228138/205037425-be7e6e98-33e4-46e2-9b13-f56b2172a673.png)
 
_____________________________Рис. 27.8. Настройка Left Wall

8.	Параметр Right Wall
 
 _____________________________![image](https://user-images.githubusercontent.com/119228138/205037394-5a08674d-624a-48b0-997a-cf724142052a.png)

 
_____________________________Рис. 27.9. Настройка Right Wall

9.	Параметр Line
 
_____________________________![image](https://user-images.githubusercontent.com/119228138/205037379-31e0518e-2d41-4039-9600-6d00287325f8.png)

 
_____________________________Рис 27.10. Настройка параметра Line

10.	Параметр GameManager
 
_____________________________![image](https://user-images.githubusercontent.com/119228138/205037664-fa5f7fa8-ec37-4154-82ed-0982c70ec05a.png)
 
_____________________________Рис. 27.11. Настройка параметра GameManager

11.	Параметр EventSystem
 
_____________________________![image](https://user-images.githubusercontent.com/119228138/205037713-59a766eb-1be3-4996-8bef-88880c6f88e1.png)
 
_____________________________Рис. 27.12. Настройка параметра EventSystem

12.	Параметры Canvas и его дочерние параметры
 
_____________________________![image](https://user-images.githubusercontent.com/119228138/205037740-d50d2fbb-1653-4612-9120-e2345501fc92.png)
 
_____________________________Рис. 27.13. Параметры Canvas и его дочерние параметры

_____________________________![image](https://user-images.githubusercontent.com/119228138/205037771-05a50479-4b46-4535-b3d9-cbd62e163438.png)
 
_____________________________Рис. 27.14. Параметры playerscore

_____________________________![image](https://user-images.githubusercontent.com/119228138/205037791-5d684647-f96c-4217-8eaa-7ec1c969b767.png)

_____________________________Рис. 27.15. Параметры Computerscore

```
1.	Cкрипт Ball
public class Ball : MonoBehaviour
{
    public float speed = 300.0f;
    private Rigidbody2D _rigidbody;
    private void Awake()
    {
        _rigidbody = GetComponent<Rigidbody2D>();
    }	
    void Start()
    {
        ResetPosition();
        AddStartingForce();
    }
    public void ResetPosition()
    {
        _rigidbody.position = Vector3.zero;
        _rigidbody.velocity = Vector3.zero;
    }
    public void AddStartingForce()
    {
        float x = Random.value < 0.5f ? -0.1f : 1.0f;
        float y = Random.value < 0.5f ? Random.Range(-1.0f, -0.5f) :
                                        Random.Range(0.5f, 1.0f);
        Vector2 direction = new Vector2(x, y);
        _rigidbody.AddForce(direction * this.speed);
    }
    public void AddForce(Vector2 force)
    {
        _rigidbody.AddForce(force);
    }
}

2.	Cкрипт BallSurface
using UnityEngine;
public class BallSurface : MonoBehaviour
{
    public float bounceStrength;
    private void OnCollisionEnter2D(Collision2D collision)
    {
        Ball ball = collision.gameObject.GetComponent<Ball>();
        if (ball != null)
        {
            Vector2 normal = collision.GetContact(0).normal;
            ball.AddForce(-normal * this.bounceStrength);
        }
    }
}

3.	Cкрипт ComputerPaddle 
public class ComputerPaddle : Paddle
{
    public Rigidbody2D ball;
    private void FixedUpdate()
    {
        if (this.ball.velocity.x > 0.0f)
        {
            if (this.ball.position.y > this.transform.position.y)
            {
                _rigidbody.AddForce(Vector2.up * this.speed);
            } 
            else if (this.ball.position.y < this.transform.position.y)
            {
                _rigidbody.AddForce(Vector2.down * this.speed);
            }
        }
        else
        {
            if (this.transform.position.y > 0.0f)
            {
                _rigidbody.AddForce(Vector2.down * this.speed);
            }
            else if (this.transform.position.y < 0.0f)
            {
                _rigidbody.AddForce(Vector2.up * this.speed);
            }
        }
    }
}

4.	Скрипт GameManager 
public class GameManager : MonoBehaviour
{
    public Ball ball;
    public Paddle computerPaddle;
    public Paddle playerPaddle;
    public TextMeshProUGUI PlayerScoreText;
    public TextMeshProUGUI computerScoreText;
    private int _playerScore;
    private int _computerScore;
    public void PlayerScores()
    {
        _playerScore++;
        this.PlayerScoreText.text = _playerScore.ToString();
        ResetRound();
       
    }
    public void ComputerScores()
    {
        _computerScore++;
        this.computerScoreText.text = _computerScore.ToString();
        ResetRound();
    }
    private void ResetRound()
    {
        this.computerPaddle.ResetPosition();
        this.playerPaddle.ResetPosition();
        this.ball.ResetPosition();
        this.ball.AddStartingForce();
    }
}

5.	Cкрипт Paddle 
public class Paddle : MonoBehaviour
{
    public float speed = 10.0f;
    protected Rigidbody2D _rigidbody;
    private void Awake()
    {
        _rigidbody = GetComponent<Rigidbody2D>();
    }
    public void ResetPosition()
    {
        _rigidbody.position = new Vector2(_rigidbody.position.x, 0.0f);
        _rigidbody.velocity = Vector2.zero;
    }
}

6.	Скрипт PlayerPaddle 
public class PlayerPaddle : Paddle
{
    private Vector2 _direction;
    private void Update()
    {
        if (Input.GetKey(KeyCode.W) || Input.GetKey(KeyCode.UpArrow))
        {
            _direction = Vector2.up;
        }
        else if (Input.GetKey(KeyCode.S) || Input.GetKey(KeyCode.DownArrow))
        {
            _direction = Vector2.down;
        }
        else
        {
            _direction = Vector2.zero;
        }
    }
    private void FixedUpdate()
    {
        if (_direction.sqrMagnitude != 0)
        {
            _rigidbody.AddForce(_direction * this.speed);
        }
    }
}

7.	Скрипт ScoringZone 
public class ScoringZone : MonoBehaviour
{
    public EventTrigger.TriggerEvent scoreTrigger;
    private void OnCollisionEnter2D(Collision2D collision)
    {
        Ball ball = collision.gameObject.GetComponent<Ball>();
        if (ball != null)
        {
            BaseEventData eventData = new BaseEventData(EventSystem.current);
            this.scoreTrigger.Invoke(eventData);
        }
    }
}
```
Вывод:В ходе выполненной работы была разработана игра ping pong

[ЛР27. Разработка игры пинг-понг.zip](https://github.com/TimurMigunov/LR27/files/10131788/27.-.zip)
