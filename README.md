# Projeto-Konkap-The-Game
game criado para a 1° gamejam na Fatenp (24/05 a 25/05/2016)

>>PLAYER

public class Player : MonoBehaviour {
    Rigidbody2D rb;
    public float velocity;
    public static int pontos, vida;
    int qndInicialive = 5;
    int maxdePontos = 20;

    public Text pontosHUD;

	// Use this for initialization
	void Start () {
        rb = GetComponent<Rigidbody2D>();
        vida = qndInicialive;
	}
		// Update is called once per frame
	void Update () {
        //Movimentar o PlayerLixeiras
        rb.velocity = new Vector2( Input.GetAxis("Horizontal") * velocity, Input.GetAxis("Vertical") * velocity);
        //Ganhar o jogo 
        if (pontos == maxdePontos)
        {
            vida++;
            maxdePontos += maxdePontos;
            // Debug.Log("vida " + maxdePontos + " " + vida);
        }
        //Game Over
        if (vida == 0)     
        {
            //pular para cena de Game Over 
            Application.LoadLevel("GameOver");
        }
        pontosHUD.text = pontos.ToString();
	}
}

>>GAMEMAKER

public class MecanicasGamePlay : MonoBehaviour {

    public float LimiteMax, LimiteMin;
    public GameObject [] objetos;
    int contador = 250;

    void Update() {      
        if (contador == 250)
        {
            Instantiate(objetos[Random.Range(0, objetos.Length)], new Vector3(
                Random.Range(LimiteMin, LimiteMax), 5.5f, 0f), Quaternion.identity);
            contador = 0;
        }
        else
        {
            contador++;
        }
    }
}

>>INIMIGOS

public class Inimigos : MonoBehaviour {

    public string Tag; 
	
	void Update () {
	}
    void OnTriggerEnter2D(Collider2D col)
    {
        //verificar a Tag
            if (col.transform.tag == Tag)
            {
                Player.pontos++;
            }
            else
            {
                Player.vida--;
            }
        Destroy(gameObject);
    }
}

>>CONTADORVIDA

public class ControleVida : MonoBehaviour {

    public int controleVida;

	void Start () {
        gameObject.SetActive(true);
    }
	
	void Update () {
	
        if(Player.vida < controleVida)
        {
            gameObject.SetActive(false);
        }
        else //if (Player.vida == controleVida)
        {
            //Debug.Log("nova vida");
            gameObject.SetActive(true);
        }
	}
}
