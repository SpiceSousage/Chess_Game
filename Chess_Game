Код главного меню
using UnityEngine;
using System.Collections;

public class Main_menu : MonoBehaviour 
{
    public GameObject _ui_start;    //Start_ui
    public GameObject _ui_exit;     //Exit_ui
    public GameObject _ui_play;     //Play_ui
    public GameObject _ui_back;     //Back_ui
    public GameObject _ui_play_black; // начать игру за черных
    public GameObject _ui_play_white; // начать игру за белых

    public GameObject stateOBJ;

   void Awake()
   {
        stateOBJ = GameObject.Find("StateMessanger"); //вызывает менеджер сцен
   }
  
   public void Exit_Game()
   {
        Application.Quit();
   }

   public void Start_Game()
   {
       _ui_start.SetActive(false);
       _ui_exit.SetActive(false);

       _ui_back.SetActive(true);

       _ui_play_black.SetActive(true);
       _ui_play_white.SetActive(true);
   }

   public void Play_Game()
   {
       Application.LoadLevel("Scene");
   }

   public void Back_to_menu()
   {
       _ui_start.SetActive(true);
       _ui_exit.SetActive(true);

       _ui_play.SetActive(false);
       _ui_back.SetActive(false);

       _ui_play_black.SetActive(false);

ui_play_white.SetActive(false);
   }
   
   public void PlayAsBlack()
   {
       SenderState scriptToAccess = stateOBJ.GetComponent<SenderState>();
       scriptToAccess.SetColor(1);
       Debug.Log("Color eq 1");
       Application.LoadLevel("Scene");
   }
   
   public void PlayAsWhite()
   {
       SenderState scriptToAccess = stateOBJ.GetComponent<SenderState>();
       scriptToAccess.SetColor(0);
       Debug.Log("Color eq 0");
       Application.LoadLevel("Scene");
   }
}
Скрипт ИИ
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class AI : MonoBehaviour {

    public GameObject Core_object;

    public List<move> All_mv = new List<move>(); // доступные ходы ИИ
    public List<move> P_All_mv = new List<move>(); // доступные ходы игроку
    public List<move> Atck_mv = new List<move>(); // массив с теми когоможно скушать
    public List<move> Move_mv = new List<move>(); // массив с теми когоможно скушать

    int cost_of_eaten = 0;
    int from_x;
    int from_z;
    int to_x;
    int to_z;

    public int[,] board = new int[8, 8]; // доска с оценкой для клеток

    private int cost_pawn = 10;    // ценность фигур
    private int cost_knight = 30;
    private int cost_bishop = 30;
    private int cost_rook = 45;
    private int queen = 95;
    private int king = 1000000;

    private int cost_m_pawn = 20;    // ценность движения
    private int cost_m_knight = 30;
    private int cost_m_bishop = 15;
    private int cost_m_rook = 15;
    private int m_queen = 15;
    private int m_king = 5;

    int cost_f; // ценность фигуры
    int started_cost_f;

    int cost_m; // ценность хода
    int started_cost_m; 

    bool danger = false; // в опасности ли король?

    public List<figure> first = new List<figure>();
    
  public void StartThinking()
  {
     from_z = 0;
     from_x = 0;
     to_z = 0;
     to_x = 0;

     checkForMovement();    // собираем массив ходов
     set_weight_of_board();
     calculate();
  }

  public void calculate() //вычисление движения фигуры
  {
     danger = false;
     cost_of_eaten = 0;

     Core scriptToAccess = Core_object.GetComponent<Core>();
     
     for (int j = 0; j < P_All_mv.Count; j++)
     {
         if (scriptToAccess.board[P_All_mv[j].z, P_All_mv[j].x].figure_name == "king" & scriptToAccess.board[P_All_mv[j].z, P_All_mv[j].x].colors_of_figure == 1)
         {
             Debug.Log("KingInDanger!!");
             Debug.Log(P_All_mv[j].z);
             Debug.Log(P_All_mv[j].x);
             Debug.Log(P_All_mv[j].started_z);
             Debug.Log(P_All_mv[j].started_x);

             danger = true;
         }
     }

     for (int i = 0; i < All_mv.Count; i++)
     {
         if (danger)
         {
             
         }
         else
         {
             if (scriptToAccess.board[All_mv[i].z, All_mv[i].x].figure_name != "empty")
             {
                 Atck_mv.Add(All_mv[i]);    // добавляем того, кого можем атаковать в массив
                 Debug.Log("added_to_attack");
             }
             else
             {
                 Move_mv.Add(All_mv[i]);    //  ход
                Debug.Log("added_to_move");
             }
         }
     }

     if (Atck_mv.Count > 0) // смотрим кого мы можем атаковать
     {
         for (int i = 0; i < Atck_mv.Count; i++)
         {
             if (scriptToAccess.board[Atck_mv[i].z, Atck_mv[i].x].figure_name == "pawn")
             {
                 if (cost_of_eaten <= cost_pawn)
                 {
                     cost_of_eaten = cost_pawn;
                     from_x = Atck_mv[i].started_x;
                     from_z = Atck_mv[i].started_z;

                     to_x = Atck_mv[i].x;
                     to_z = Atck_mv[i].z;
                 }
             }

             if (scriptToAccess.board[Atck_mv[i].z, Atck_mv[i].x].figure_name == "knight")
             {
                 if (cost_of_eaten <= cost_knight)
                 {
                     cost_of_eaten = cost_knight;
                     from_x = Atck_mv[i].started_x;
                     from_z = Atck_mv[i].started_z;

                     to_x = Atck_mv[i].x;
                     to_z = Atck_mv[i].z;
                 }
             }

             if (scriptToAccess.board[Atck_mv[i].z, Atck_mv[i].x].figure_name == "king")
             {
                     from_x = Atck_mv[i].started_x;
                     from_z = Atck_mv[i].started_z;

                     to_x = Atck_mv[i].x;
                     to_z = Atck_mv[i].z;

                     break;
             }

             if (scriptToAccess.board[Atck_mv[i].z, Atck_mv[i].x].figure_name == "bishop")
             {
                 if (cost_of_eaten <= cost_bishop)
                 {
                     cost_of_eaten = cost_bishop;
                     from_x = Atck_mv[i].started_x;
                     from_z = Atck_mv[i].started_z;

                     to_x = Atck_mv[i].x;
                     to_z = Atck_mv[i].z;
                 }
             }

             if (scriptToAccess.board[Atck_mv[i].z, Atck_mv[i].x].figure_name == "queen")
             {
                 if (cost_of_eaten <= queen)
                 {
                     cost_of_eaten = queen;
                     from_x = Atck_mv[i].started_x;
                     from_z = Atck_mv[i].started_z;

                     to_x = Atck_mv[i].x;
                     to_z = Atck_mv[i].z;
                 }
             }

             if (scriptToAccess.board[Atck_mv[i].z, Atck_mv[i].x].figure_name == "rook")
             {
                 if (cost_of_eaten <= cost_rook)
                 {
                     cost_of_eaten = cost_rook;
                     from_x = Atck_mv[i].started_x;
                     from_z = Atck_mv[i].started_z;

                     to_x = Atck_mv[i].x;
                     to_z = Atck_mv[i].z;
                 }
             }
         }
         /*
         Debug.Log("from and to");
         Debug.Log(from_z);
         Debug.Log(from_x);
         Debug.Log(to_z);
         Debug.Log(to_x);
         Debug.Log("trying to eat");
         */
         MOVEForAI(from_z, from_x, to_z, to_x); // идем кушать
     }
     else
     {
        // Debug.Log(All_mv.Count);
         for (int i = 0; i < All_mv.Count; i++)
         {
             started_cost_f = 0;
             started_cost_m = 0;

             if (scriptToAccess.board[Move_mv[i].started_z, Move_mv[i].started_x].figure_name == "pawn")
             {
                 started_cost_m = cost_m_pawn + board[Move_mv[i].z, Move_mv[i].x];
                // Debug.Log("cost");
                // Debug.Log(started_cost_m);
             }

             if (scriptToAccess.board[Move_mv[i].started_z, Move_mv[i].started_x].figure_name == "knight")
             {
                 started_cost_m = cost_m_knight + board[Move_mv[i].z, Move_mv[i].x];
               // Debug.Log("cost");
               // Debug.Log(started_cost_m);
             }

             if (scriptToAccess.board[Move_mv[i].started_z, Move_mv[i].started_x].figure_name == "king")
             {
                 started_cost_m = m_king + board[Move_mv[i].z, Move_mv[i].x];
                // Debug.Log("cost");
                // Debug.Log(started_cost_m);
             }

             if (scriptToAccess.board[Move_mv[i].started_z, Move_mv[i].started_x].figure_name == "bishop")
             {
                 started_cost_m = cost_m_bishop + board[Move_mv[i].z, Move_mv[i].x];
                // Debug.Log("cost");
                // Debug.Log(started_cost_m);
             }

             if (scriptToAccess.board[Move_mv[i].started_z, Move_mv[i].started_x].figure_name == "bishop")
             {
                 started_cost_m = cost_m_bishop + board[Move_mv[i].z, Move_mv[i].x];
               // Debug.Log("cost");
               // Debug.Log(started_cost_m);
             }

             if (scriptToAccess.board[Move_mv[i].started_z, Move_mv[i].started_x].figure_name == "queen")
             {
                 started_cost_m = m_queen + board[Move_mv[i].z, Move_mv[i].x];
               // Debug.Log("cost");
               // Debug.Log(started_cost_m);
             }

             if (scriptToAccess.board[Move_mv[i].started_z, Move_mv[i].started_x].figure_name == "rook")
             {
                 started_cost_m = m_queen + board[Move_mv[i].z, Move_mv[i].x];
               // Debug.Log("cost");
               // Debug.Log(started_cost_m);
             }

             if (cost_m <= started_cost_m)
             {
                 cost_m = started_cost_m;
                 from_x = Move_mv[i].started_x;
                 from_z = Move_mv[i].started_z;

                 to_x = Move_mv[i].x;
                 to_z = Move_mv[i].z;
             }
         }
         /*
         Debug.Log("from and to");
         Debug.Log(from_z);
         Debug.Log(from_x);
         Debug.Log(to_z);
         Debug.Log(to_x);
         Debug.Log("trying to move");
         */     
         if (from_z == 0 & from_x == 0 & to_z == 0 & to_x == 0)
         {
             Debug.Log("cant do this");

              cost_of_eaten = 0;
              from_x = 0;
              from_z = 0;
              to_x = 0;
              to_z = 0;

               cost_f = 0;
               started_cost_f = 0;
               cost_m = 0;
               started_cost_m = 0;

               All_mv.Clear();
               P_All_mv.Clear();
               Atck_mv.Clear();
               Move_mv.Clear();

               StartThinking();
         }
         else
         {
             MOVEForAI(from_z, from_x, to_z, to_x);
         }
     }
  }
 
    public void set_weight_of_board() // Ставится приоритет для доски
    {
      Core scriptToAccess = Core_object.GetComponent<Core>();

      if (scriptToAccess.moves < 5)
      {
          for (int i = 0; i < 8; i++)
          {
              board[7, i] = 10;
              board[6, i] = 10;
              board[5, i] = 20;
              board[4, i] = 25;
              board[3, i] = 10;
              board[2, i] = 10;
              board[1, i] = 10;
              board[0, i] = 10;
          }
      }

      if (scriptToAccess.moves > 5)
      {
          for (int i = 0; i < 8; i++)
          {
              board[7, i] = 10;
              board[6, i] = 10;
              board[5, i] = 20;
              board[4, i] = 20;
              board[3, i] = 30;
              board[2, i] = 40;
              board[1, i] = 50;
              board[0, i] = 60;
          }
      }
      Debug.Log("settled");
    }

   public void checkForMovement() // собирает все возможные ходы
   {
     Core scriptToAccess = Core_object.GetComponent<Core>();

     for (int i = 0; i < 8; i++)
     {
         for (int j = 0; j < 8; j++)
         {
             if (scriptToAccess.board[i, j].figure_name == "bishop")
             {
                 if (scriptToAccess.board[i, j].colors_of_figure == 1)
                 {

                     bishop b = new bishop();
                     b.colors_of_figure = 1;
                     b.PossibleMoves(i, j);
                     for (int x = 0; x < b.All_moves.Count; x++)
                     {
                         Debug.Log("bishops");
                         All_mv.Add(b.All_moves[x]);
                     }
                 }
                 else   // иначе мы смотрим ход для игрока
                 {
                     bishop b = new bishop();
                     b.colors_of_figure = 0;
                     b.PossibleMoves(i, j);

                     for (int x = 0; x < b.All_moves.Count; x++)
                     {
                         // Debug.Log("bishops");
                         // P_All_mv.Add(b.All_moves[x]);
                     }
                 }
             }

             if (scriptToAccess.board[i, j].figure_name == "king")
             {
                 if (scriptToAccess.board[i, j].colors_of_figure == 1)
                 {

                     king b = new king();
                     b.colors_of_figure = 1;
                     b.PossibleMoves(i, j);

                     for (int x = 0; x < b.P_Moves.Count; x++)
                     {
                         Debug.Log("king");
                         All_mv.Add(b.P_Moves[x]);
                     }
                 }
                 else   // иначе мы смотрим ход для игрока
                 {
                     king b = new king();
                     b.colors_of_figure = 1;
                     b.PossibleMoves(i, j);

                     for (int x = 0; x < b.P_Moves.Count; x++)
                     {
                         // P_All_mv.Add(b.P_Moves[x]);
                     }
                 }
             }

             if (scriptToAccess.board[i, j].figure_name == "knight")
             {
                 if (scriptToAccess.board[i, j].colors_of_figure == 1)
                 {
                     knight b = new knight();
                     b.colors_of_figure = 1;
                     b.PossibleMoves(i, j);

                     for (int x = 0; x < b.P_Moves.Count; x++)
                     {
                                 Debug.Log("knight");
                                 Debug.Log(b.P_Moves[x].z);
                                 Debug.Log(b.P_Moves[x].x);

                         All_mv.Add(b.P_Moves[x]);

                         //  Debug.Log(All_mv.Count);
                         //  Debug.Log(scriptToAccess.board[0, 6].colors_of_figure);
                     }
                 }
                 else
                 {
                     knight b = new knight();
                     b.colors_of_figure = 0;
                     b.PossibleMoves(i, j);

                     for (int x = 0; x < b.P_Moves.Count; x++)
                     {
                          //  Debug.Log("knight");
                          //  P_All_mv.Add(b.P_Moves[x]);
                          //  Debug.Log(scriptToAccess.board[0, 6].colors_of_figure);
                     }
                 }
             }

             if (scriptToAccess.board[i, j].figure_name == "pawn")
             {
                 if (scriptToAccess.board[i, j].colors_of_figure == 1)
                 {
                     pawn b = new pawn();
                     b.colors_of_figure = 1;
                     b.PossibleMovesAI(i, j);

                     for (int x = 0; x < b.P_Moves.Count; x++)
                     {
                         Debug.Log("pawn");
                         All_mv.Add(b.P_Moves[x]);
                     }

                     for (int x = 0; x < b.Attack_Moves.Count; x++)
                     {
                         Debug.Log("pawn");
                         All_mv.Add(b.Attack_Moves[x]);
                     }
                 }
                 else
                 {
                     pawn b = new pawn();
                     b.colors_of_figure = 0;
                     b.PossibleMoves(i, j);
                     for (int x = 0; x < b.P_Moves.Count; x++)
                     {
                        // P_All_mv.Add(b.P_Moves[x]);
                     }

                     for (int x = 0; x < b.Attack_Moves.Count; x++)
                     {
                        // P_All_mv.Add(b.Attack_Moves[x]);
                     }
                 }
             }

             if (scriptToAccess.board[i, j].figure_name == "queen")
             {
                 if (scriptToAccess.board[i, j].colors_of_figure == 1)
                 {
                     queen b = new queen();
                     b.colors_of_figure = 1;
                     b.PossibleMoves(i, j);

                     for (int x = 0; x < b.All_moves.Count; x++)
                     {
                         Debug.Log("queen");
                         All_mv.Add(b.All_moves[x]);
                     }
                 }
                 else   // иначе мы смотрим ход для игрока
                 {
                     queen b = new queen();
                     b.colors_of_figure = 0;
                     b.PossibleMoves(i, j);

                     for (int x = 0; x < b.All_moves.Count; x++)
                     {
                        // Debug.Log("queen");
                        // P_All_mv.Add(b.All_moves[x]);
                     }
                 }
             }

             if (scriptToAccess.board[i, j].figure_name == "rook")
             {
                 if (scriptToAccess.board[i, j].colors_of_figure == 1)
                 {
                     rook b = new rook();
                     b.colors_of_figure = 1;
                     b.PossibleMoves(i, j);

                     for (int x = 0; x < b.All_moves.Count; x++)
                     {
                         Debug.Log("rook");
                         All_mv.Add(b.All_moves[x]);
                     }
                 }
                 else   // иначе мы смотрим ход для игрока
                 {
                     rook b = new rook();
                     b.colors_of_figure = 0;
                     b.PossibleMoves(i, j);

                     for (int x = 0; x < b.All_moves.Count; x++)
                     {
                        // Debug.Log("rook");
                        // P_All_mv.Add(b.All_moves[x]);
                     }
                 }
             }
         }
     }
   }

    /// <summary>
    /// запрос на передвижение или атаку
    /// <param name="z">начальная координата по z</param>
    /// <param name="x">начальная координата по x</param>
    /// <param name="second_z"> конечная координата по z </param>
    /// <param name="second_x">  конечная координата по x</param>
    /// </summary>
    
  public void MOVEForAI(int z, int x, int second_z, int second_x)    // z x координаты первой фигуры, s_z s_x координаты 2ой фигуры
  {
     All_mv.Clear();
     P_All_mv.Clear();
     Atck_mv.Clear();
     Move_mv.Clear();

     Debug.Log("MoveForAI");
    //  Debug.Log(z);
    //  Debug.Log(x);
    //  Debug.Log(second_z);
    //  Debug.Log(second_x);

     Core scriptToAccess = Core_object.GetComponent<Core>();

     if (scriptToAccess.State == 1)    // идет нужная логика где мы выбираем потенциально аенный ход и двигаем фигурки
     {
         scriptToAccess.z = z;  // координаты первой активной фигуры для движения
         scriptToAccess.x = x;

         scriptToAccess.second_z = second_z;    // координаты второй активной фигуры для движения
         scriptToAccess.second_x = second_x;

         if (scriptToAccess.board[second_z, second_x].colors_of_figure == 0)
         {
             scriptToAccess.AttackFigure();
         }
         else
         {
             scriptToAccess.MoveFigure();
         }
         
         scriptToAccess.State = 0;  // 1 - ход ИИ 0 - Ход Игрока
         Debug.Log(All_mv.Count);
     }
  }
}
Основной скрипт, что запускается вместе с запуском приложении
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
/// <summary>
/// Выделение нужных фигур пользователем (граф. часть)
/// </summary>
public class Active : MonoBehaviour
{

    public Renderer rend;
    public Material default_mat;
    public Material mat;
    public Material second_mat;
    public GameObject Core_object;
    private int first_number;
    private int second_number;

    private bool first_active;
    private bool second_active;

    /// <summary>
    /// единожды срабатывает функция в начале
    /// </summary>
    void Awake()
    {
        Core_object = GameObject.Find("Core");
        default_mat = rend.material;

        first_number = (int)this.transform.position.z;
        second_number = (int)this.transform.position.x;

    }
    /// <summary>
    /// смена материала
    /// </summary>
    void LateUpdate()
    {

        Core scriptToAccess = Core_object.GetComponent<Core>();

        for (int i = 0; i < 8; i++)
        {
            for (int j = 0; j < 8; j++)
            {

                if (scriptToAccess.board[first_number, second_number].active == false)
                {
                    rend.material = default_mat;
                }
            }
        }
        
    }


    /// <summary>
    /// событие на нажатие клавиши
    /// </summary>
    void OnMouseUp()        // будет работать только если мы белые
    {


        Core scriptToAccess = Core_object.GetComponent<Core>();

        for (int i = 0; i < 8; i++)
        {
            for (int j = 0; j < 8; j++)
            {

                if (scriptToAccess.board[first_number, second_number].active == false)
                {
                    rend.material = default_mat;
                }
            }
        }
 


        if (scriptToAccess.State == 0)
        {
            if (scriptToAccess.board[first_number, second_number].colors_of_figure == 0)
            {


                if (!first_active)  // если не выбрана первая фигура, то мы вибираем ее
                {
           
                    if (scriptToAccess.board[first_number, second_number].figure_name != "empty")   // пустая фигура не может быть выделена для движения
                    {
                        scriptToAccess.ActivateFigure(this.transform.position.z, this.transform.position.x);
                        scriptToAccess.z = (int)this.transform.position.z;
                        scriptToAccess.x = (int)this.transform.position.x;
                        first_active = true;
                        scriptToAccess.CheckFirstActive();
                        Debug.Log("activated figure is");
                        Debug.Log(scriptToAccess.board[scriptToAccess.z, scriptToAccess.x]);
                        Changing_First_Materials();

                      //  scriptToAccess.DebuggingStats(first_number, second_number);

                    }
                }
            }

            // если фигура пустая то выбираем ее как второе значение, если нет выбираем как первое и если цвет черный то как 2ую выбираем
            if (scriptToAccess.FirstActiveted)
            {
                if (scriptToAccess.board[first_number, second_number].figure_name == "empty")
                {
                    scriptToAccess.SecondActivateFigure(this.transform.position.z, this.transform.position.x);
                    scriptToAccess.second_z = (int)this.transform.position.z;
                    scriptToAccess.second_x = (int)this.transform.position.x;
                    scriptToAccess.isMoveCanBe(scriptToAccess.board[scriptToAccess.z, scriptToAccess.x].figure_name, scriptToAccess.board[first_number, second_number].colors_of_figure);
                    second_active = true;

                    
                }

                else
                {
                    if (scriptToAccess.board[first_number, second_number].colors_of_figure == 1)    // Надо вызывать атаку
                    {
                        scriptToAccess.SecondActivateFigure(this.transform.position.z, this.transform.position.x);
                        scriptToAccess.second_z = (int)this.transform.position.z;
                        scriptToAccess.second_x = (int)this.transform.position.x;
                        scriptToAccess.isMoveCanBe(scriptToAccess.board[scriptToAccess.z, scriptToAccess.x].figure_name, scriptToAccess.board[first_number, second_number].colors_of_figure);
                        second_active = true;
                        
                    }
                    else
                    {

                        scriptToAccess.ActivateFigure(this.transform.position.z, this.transform.position.x);
                        Changing_First_Materials();
                        scriptToAccess.z = (int)this.transform.position.z;
                        scriptToAccess.x = (int)this.transform.position.x;
                        first_active = true;

                    }
                }
            }   // color
            else
            {   // пишем атаку
                


            }

        }   // state

    }

    /// <summary>
    /// меняет материалы
    /// </summary>
    void Changing_First_Materials()
    {
        Core scriptToAccess = Core_object.GetComponent<Core>();
        for (int i = 0; i < 8; i++)
        {
            for (int j = 0; j < 8; j++)
            {

                rend.material = default_mat;

            }
        }

        if (scriptToAccess.board[first_number, second_number].active)
        {
            this.rend.material = mat;
        }
    }
}
Скрипт поворота камеры
using UnityEngine;
using System.Collections;

public class Rotate_Around_Scene : MonoBehaviour {

    public Transform point_of_rotating;

    public float sensitivityX, sensitivityY = 15f;
    public float speed = 10f;

    private bool entered_r = false;     //For left panel
    private bool entered_l = false;     //For right panel
    
	// Update is called once per frame
	void Update ()
    {
        Rotating();
	}

    void Rotating()
    {
        if (entered_r)
        {
            transform.RotateAround(point_of_rotating.transform.position, new Vector3(0, -1, 0), 20 * Time.deltaTime * speed);
        }

        if (entered_l)
        {
            transform.RotateAround(point_of_rotating.transform.position, new Vector3(0, 1, 0), 20 * Time.deltaTime * speed);
        }
    }

    public void OnEnterRight()
    {
        entered_r = true;
    }

    public void OnEnterLeft()
    {
        entered_l = true;
    }

    public void OnExitLeft()
    {
        entered_l = false;
    }
    public void OnExitRight()
    {
        entered_r = false;
    }
}
Скрипт пешки
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class pawn : figure
{          //пешка

    public bool direction; // true - white to black else reverse

    public string name = "pawn";
    // public bool first_move = true; // если первый шаг пешки за игру то +1 возможный ход
    public GameObject Core_object;

    public List<move> P_Moves = new List<move>();   // возможные шаги фигуры 
    public List<move> Attack_Moves = new List<move>();   // для специфичной атаки пешек

    public int for_z;
    public int for_x;

    public void PossibleMoves(int z, int x) //   28 возможных ходов
    {
        for_z = z;
        for_x = x;

        Core_object = GameObject.Find("Core");
        Core scriptToAccess = Core_object.GetComponent<Core>();

        if (scriptToAccess.State == 0)
        {
            move mv = new move();

            mv.x = x;      // для движения
            mv.z = z + 1;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)   // строчка для ограничения хода в границах 8x8 
            {
                if (scriptToAccess.board[mv.z, mv.x].figure_name == "empty")
                {
                    P_Moves.Add(mv);
                }
            }

            z = for_z;
            x = for_x;

            move mv1 = new move();

            mv1.x = x + 1;  // для атаки
            mv1.z = z + 1;

            if (mv1.x >= 0 & mv1.x < 8 & mv1.z >= 0 & mv1.z < 8)   // строчка для ограничения хода в границах 8x8 
            {
                if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != 0 & scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].figure_name != "empty")
                {
                    Attack_Moves.Add(mv1);
                }
            }

            z = for_z;
            x = for_x;

            move mv2 = new move();

            mv2.x = x - 1;  // для атаки
            mv2.z = z + 1;

            if (mv2.x >= 0 & mv2.x < 8 & mv2.z >= 0 & mv2.z < 8)   // строчка для ограничения хода в границах 8x8 
            {

                if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != 0 & scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].figure_name != "empty")
                {
                    Attack_Moves.Add(mv2);
                }
            }

            z = for_z;
            x = for_x;

            move mv3 = new move();

            if (first_move)
            {
                mv3.x = x;  // для движения
                mv3.z = z + 2;
                if (mv3.x >= 0 & mv3.x < 8 & mv3.z >= 0 & mv3.z < 8)   // строчка для ограничения хода в границах 8x8 
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name == "empty")
                    {
                        P_Moves.Add(mv3);
                    }
                }
                z = for_z;
                x = for_x;
            }
        }

        for (int i = 0; i < P_Moves.Count; i++)
        {
            P_Moves[i].name = "pawn";
        }

        for (int i = 0; i < Attack_Moves.Count; i++)
        {
            Attack_Moves[i].name = "pawn";
        }
    }

    public void PossibleMovesAI(int z, int x) //возможные ходы ИИ
    {
        int for_z, for_x;
        int myColor = 0;

        for_z = z;
        for_x = x;

        Core_object = GameObject.Find("Core");
        Core scriptToAccess = Core_object.GetComponent<Core>();

        if (scriptToAccess.State == 1)
        {

            myColor = 1;
        }
        else
        {
            myColor = 0;
        }

        if (scriptToAccess.State == 1)
        {
            move mv4 = new move();

            mv4.x = x;      // для движения
            mv4.z = z - 1;

            if (mv4.x >= 0 & mv4.x < 8 & mv4.z >= 0 & mv4.z < 8)   // строчка для ограничения хода в границах 8x8 
            {
                if (scriptToAccess.board[mv4.z, mv4.x].figure_name == "empty")
                {
                    P_Moves.Add(mv4);
                }
            }

            z = for_z;
            x = for_x;

            move mv5 = new move();

            mv5.x = x + 1;  // для атаки
            mv5.z = z - 1;

            if (mv5.x >= 0 & mv5.x < 8 & mv5.z >= 0 & mv5.z < 8)   // строчка для ограничения хода в границах 8x8 
            {
                if (scriptToAccess.board[mv5.z, mv5.x].colors_of_figure != myColor & scriptToAccess.board[mv5.z, mv5.x].figure_name != "empty")
                {
                    if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != 1)
                    {
                        Attack_Moves.Add(mv5);
                        Debug.Log("ATTAPAWN");
                    }
                }
            }

            z = for_z;
            x = for_x;

            move mv6 = new move();

            mv6.x = x - 1;  // для атаки
            mv6.z = z - 1;

            if (mv6.x >= 0 & mv6.x < 8 & mv6.z >= 0 & mv6.z < 8)   // строчка для ограничения хода в границах 8x8 
            {
                if (scriptToAccess.board[mv6.z, mv6.x].colors_of_figure != myColor & scriptToAccess.board[mv6.z, mv6.x].figure_name != "empty")
                {
                    if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != 1)
                    {
                        Attack_Moves.Add(mv6);
                        Debug.Log("ATTAPAWN");
                    }
                }

                z = for_z;
                x = for_x;

                if (first_move)
                {
                    move mv7 = new move();

                    mv7.x = x;  // для движения
                    mv7.z = z - 2;

                    if (mv7.x >= 0 & mv7.x < 8 & mv7.z >= 0 & mv7.z < 8)   // строчка для ограничения хода в границах 8x8  
                    {
                        if (mv7.z == 6)
                        {
                            P_Moves.Add(mv7);
                        }
                    }
                }

                z = for_z;
                x = for_x;
            }

            for (int i = 0; i < P_Moves.Count; i++)
            {
                P_Moves[i].name = "pawn";
                P_Moves[i].started_z = for_z;
                P_Moves[i].started_x = for_x;
            }

            for (int i = 0; i < Attack_Moves.Count; i++)
            {
                Attack_Moves[i].name = "pawn";
                Attack_Moves[i].started_z = for_z;
                Attack_Moves[i].started_x = for_x;
                
            }
            Debug.Log("ATTACOUNT");
            Debug.Log(Attack_Moves.Count);
        }
    }
}
Скрипт коня
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class knight : figure
{           //конь

    public string name = "knight";
    public bool ischeck = false;

    public List<move> P_Moves = new List<move>();   //возможные шаги фигуры 
    public GameObject Core_object;

    public void PossibleMoves(int z, int x) //   28 возможных ходов
    {
        Core_object = GameObject.Find("Core");
        Core scriptToAccess = Core_object.GetComponent<Core>();

        int myColor = 0;

        if (scriptToAccess.State == 1)
        {

            myColor = 1;
        }
        else
        {
            myColor = 0;
        }

        int for_z, for_x;
        for_z = z;
        for_x = x;

        move mv = new move();

        mv.x = x - 2;
        mv.z = z + 1;

        if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)
        {   // строчка для ограничения хода в границах 8x8 
            if (scriptToAccess.board[mv.z, mv.x].colors_of_figure != myColor || scriptToAccess.board[mv.z, mv.x].figure_name == "empty")
            {
                P_Moves.Add(mv);
            }
        }

        z = for_z;
        x = for_x;

        move mv1 = new move();
        mv1.x = x - 1;
        mv1.z = z + 2;

        if (mv1.x >= 0 & mv1.x < 8 & mv1.z >= 0 & mv1.z < 8)
        {   // строчка для ограничения хода в границах 8x8 
            if (scriptToAccess.board[mv1.z, mv1.x].colors_of_figure != myColor || scriptToAccess.board[mv1.z, mv1.x].figure_name == "empty")
            {
                P_Moves.Add(mv1);
            }
        }

        z = for_z;
        x = for_x;

        move mv2 = new move();
        mv2.x = x + 1;
        mv2.z = z + 2;

        if (mv2.x >= 0 & mv2.x < 8 & mv2.z >= 0 & mv2.z < 8)
        {   // строчка для ограничения хода в границах 8x8 
            if (scriptToAccess.board[mv2.z, mv2.x].colors_of_figure != myColor || scriptToAccess.board[mv2.z, mv2.x].figure_name == "empty")
            {
                P_Moves.Add(mv2);
            }
        }

        z = for_z;
        x = for_x;

        move mv3 = new move();
        mv3.x = x + 2;
        mv3.z = z + 1;

        if (mv3.x >= 0 & mv3.x < 8 & mv3.z >= 0 & mv3.z < 8)
        {  // строчка для ограничения хода в границах 8x8 
            if (scriptToAccess.board[mv3.z, mv3.x].colors_of_figure != myColor || scriptToAccess.board[mv3.z, mv3.x].figure_name == "empty")
            {
                P_Moves.Add(mv3);
            }
        }

        z = for_z;
        x = for_x;

        move mv4 = new move();
        mv4.x = x + 2;
        mv4.z = z - 1;

        if (mv4.x >= 0 & mv4.x < 8 & mv4.z >= 0 & mv4.z < 8)
        {   // строчка для ограничения хода в границах 8x8

            if (scriptToAccess.board[mv4.z, mv4.x].colors_of_figure != myColor || scriptToAccess.board[mv4.z, mv4.x].figure_name == "empty")
            {
                P_Moves.Add(mv4);
            }
        }

        z = for_z;
        x = for_x;

        move mv5 = new move();
        mv5.x = x + 1;
        mv5.z = z - 2;

        if (mv5.x >= 0 & mv5.x < 8 & mv5.z >= 0 & mv5.z < 8)
        {   // строчка для ограничения хода в границах 8x8 
            if (scriptToAccess.board[mv5.z, mv5.x].colors_of_figure != myColor || scriptToAccess.board[mv5.z, mv5.x].figure_name == "empty")
            {
                P_Moves.Add(mv5);
            }
        }

        z = for_z;
        x = for_x;

        move mv6 = new move();
        mv6.x = x - 1;
        mv6.z = z - 2;

        if (mv6.x >= 0 & mv6.x < 8 & mv6.z >= 0 & mv6.z < 8)
        { // строчка для ограничения хода в границах 8x8 
            if (scriptToAccess.board[mv6.z, mv6.x].colors_of_figure != myColor || scriptToAccess.board[mv6.z, mv6.x].figure_name == "empty")
            {
                P_Moves.Add(mv6);
            }
        }

        z = for_z;
        x = for_x;

        move mv7 = new move();
        mv7.x = x - 2;
        mv7.z = z - 1;

        if (mv7.x >= 0 & mv7.x < 8 & mv7.z >= 0 & mv7.z < 8)
        {  // строчка для ограничения хода в границах 8x8 
            if (scriptToAccess.board[mv7.z, mv7.x].colors_of_figure != myColor || scriptToAccess.board[mv7.z, mv7.x].figure_name == "empty")
            {
                P_Moves.Add(mv7);
            }
        }
        for (int i = 0; i < P_Moves.Count; i++)
        {
            P_Moves[i].name = "knight";
            P_Moves[i].started_z = for_z;
            P_Moves[i].started_x = for_x;
        }
    }
}
Скрипт короля
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class king : figure 
{          //король
    public GameObject Core_object;

    public bool isCheck = false;
    public bool isMate = false;

    public string name = "king";

    public List<move> P_Moves = new List<move>();   // возможные шаги фигуры 
    public List<move> P_Moves_r = new List<move>();   // ходы для рокировки (всего 2 вида - длинная/короткая)

    public void PossibleMoves(int z, int x) // 8 возможных ходов + рокировка(похоже работает)
    {
        int for_z, for_x;
        for_z = z;
        for_x = x;

        Core_object = GameObject.Find("Core");
        Core scriptToAccess = Core_object.GetComponent<Core>(); //для проверки налиия пустых клеток для рокировки

        int myColor = 0;

        if (scriptToAccess.State == 1)
        {
            myColor = 1;
        }
        else
        {
            myColor = 0;
        }

        if(scriptToAccess.board[0,4].colors_of_figure == 0)    // рокировка для белого короля
        {
            if (z == 0 & x == 4)
            {
                if (scriptToAccess.board[0, 2].figure_name == "empty" & scriptToAccess.board[0, 3].figure_name == "empty" & scriptToAccess.board[0, 0].figure_name == "rook")//long
                {
                    z = for_z;
                    x = for_x;

                    move lv = new move();
                    lv.x = x - 2;
                    lv.z = z;
                    P_Moves_r.Add(lv);
                }

                z = for_z;
                x = for_x;

                if (scriptToAccess.board[0, 5].figure_name == "empty" & scriptToAccess.board[0, 6].figure_name == "empty" & scriptToAccess.board[0, 7].figure_name == "rook")//short
                {
                    move sv = new move();
                    sv.x = x + 2;
                    sv.z = z;
                    P_Moves_r.Add(sv);
                }

                z = for_z;
                x = for_x;
            }
        }

        if (scriptToAccess.board[0, 4].colors_of_figure == 1)   // рокировка для чернрго короля
        {
            if (z == 7 & x == 4)
            {
                if (scriptToAccess.board[7, 2].figure_name == "empty" & scriptToAccess.board[7, 3].figure_name == "empty")//long
                {
                    z = for_z;
                    x = for_x;

                    move lv = new move();
                    lv.x = x - 2;
                    lv.z = z;
                    P_Moves_r.Add(lv);
                }

                if (scriptToAccess.board[7, 5].figure_name == "empty" & scriptToAccess.board[7, 6].figure_name == "empty")//short
                {
                    z = for_z;
                    x = for_x;

                    move sv = new move();
                    sv.x = x + 2;
                    sv.z = z;
                    P_Moves_r.Add(sv);
                }
            }
        }

        z = for_z;
        x = for_x;

        move mv = new move();
        mv.x = x + 1;       // право-верх направление
        mv.z = z + 1;

        if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)   // строчка для ограничения хода в границах 8x8 
        {
            if (scriptToAccess.board[mv.z, mv.x].colors_of_figure != myColor || scriptToAccess.board[mv.z, mv.x].figure_name == "empty")
            {
                 P_Moves.Add(mv);
            }
        }

        z = for_z;
        x = for_x;

        move mv1 = new move();
        mv1.x = x + 1;       // право направление
        mv1.z = z;

        if (mv1.x >= 0 & mv1.x < 8 & mv1.z >= 0 & mv1.z < 8)   // строчка для ограничения хода в границах 8x8 
        {
            if (scriptToAccess.board[mv1.z, mv1.x].colors_of_figure != myColor || scriptToAccess.board[mv1.z, mv1.x].figure_name == "empty")
            {
                P_Moves.Add(mv1);
                //    Debug.Log("road_to");
                //    Debug.Log(scriptToAccess.board[mv1.z, mv1.x].colors_of_figure);
                //    Debug.Log(scriptToAccess.board[mv1.z, mv1.x].figure_name);
                //    Debug.Log("from");
                //    Debug.Log(scriptToAccess.board[for_z, for_x].colors_of_figure);
                //    Debug.Log(scriptToAccess.board[for_z, for_x].figure_name);
                //    Debug.Log("myolor");
                //    Debug.Log(mv1.z);
                //    Debug.Log(mv1.x);
                //    Debug.Log("king");
            }
        }

        z = for_z;
        x = for_x;

        move mv2 = new move();
        mv2.x = x + 1;       // право-вниз направление
        mv2.z = z - 1;

        if (mv2.x >= 0 & mv2.x < 8 & mv2.z >= 0 & mv2.z < 8)   // строчка для ограничения хода в границах 8x8 
        {
            if (scriptToAccess.board[mv2.z, mv2.x].colors_of_figure != myColor || scriptToAccess.board[mv2.z, mv2.x].figure_name == "empty")
            {
                P_Moves.Add(mv2);
          //      Debug.Log(mv2.z);
          //      Debug.Log(mv2.x);
          //      Debug.Log("king");
            }
        }

        z = for_z;
        x = for_x;

        move mv3 = new move();
        mv3.x = x;       // вниз направление
        mv3.z = z - 1;
        if (mv3.x >= 0 & mv3.x < 8 & mv3.z >= 0 & mv3.z < 8)   // строчка для ограничения хода в границах 8x8 
        {
            if (scriptToAccess.board[mv3.z, mv3.x].colors_of_figure != myColor || scriptToAccess.board[mv3.z, mv3.x].figure_name == "empty")
            {
                P_Moves.Add(mv3);
           //    Debug.Log(mv3.z);
           //    Debug.Log(mv3.x);
           //    Debug.Log("king");
            }
        }

        z = for_z;
        x = for_x;

        move mv4 = new move();
        mv4.x = x - 1;       // влево-вниз направление
        mv4.z = z - 1;
        if (mv4.x >= 0 & mv4.x < 8 & mv4.z >= 0 & mv4.z < 8)   // строчка для ограничения хода в границах 8x8 
        {
            if (scriptToAccess.board[mv4.z, mv4.x].colors_of_figure != myColor || scriptToAccess.board[mv4.z, mv4.x].figure_name == "empty")
            {
                P_Moves.Add(mv4);
           //   Debug.Log(mv4.z);
           //   Debug.Log(mv4.x);
           //   Debug.Log("king");
            }
        }

        z = for_z;
        x = for_x;

        move mv5 = new move();
        mv5.x = x - 1;       // влево направление
        mv5.z = z;

        if (mv5.x >= 0 & mv5.x < 8 & mv5.z >= 0 & mv5.z < 8)   // строчка для ограничения хода в границах 8x8 
        {
            if (scriptToAccess.board[mv5.z, mv5.x].colors_of_figure != myColor || scriptToAccess.board[mv5.z, mv5.x].figure_name == "empty")
            {
                P_Moves.Add(mv5);
             // Debug.Log(mv5.z);
             // Debug.Log(mv5.x);
             // Debug.Log("king");
            }
        }

        z = for_z;
        x = for_x;

        move mv6 = new move();
        mv6.x = x - 1;       // влево-вверх направление
        mv6.z = z + 1;

        if (mv6.x >= 0 & mv6.x < 8 & mv6.z >= 0 & mv6.z < 8)   // строчка для ограничения хода в границах 8x8 
        {
            if (scriptToAccess.board[mv6.z, mv6.x].colors_of_figure != myColor || scriptToAccess.board[mv6.z, mv6.x].figure_name == "empty")
            {
                P_Moves.Add(mv6);
            }
        }

        z = for_z;
        x = for_x;

        move mv7 = new move();
        mv7.x = x;       // вверх направление
        mv7.z = z + 1;

        if (mv7.x >= 0 & mv7.x < 8 & mv7.z >= 0 & mv7.z < 8)   // строчка для ограничения хода в границах 8x8 
        {
            if (scriptToAccess.board[mv7.z, mv7.x].colors_of_figure != myColor || scriptToAccess.board[mv7.z, mv7.x].figure_name == "empty")
            {
                P_Moves.Add(mv7);;
            }
        }

        for (int i = 0; i < P_Moves.Count; i++)
        {
            P_Moves[i].name = "king";
            P_Moves[i].started_z = for_z;
            P_Moves[i].started_x = for_x;
        }

        for (int i = 0; i < P_Moves_r.Count; i++)
        {
            P_Moves_r[i].name = "king";
            P_Moves_r[i].started_z = for_z;
            P_Moves_r[i].started_x = for_x;
        }
    }
}
Скрипт ферзя
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class queen : figure
{         // королева

    public string name = "queen";

    public List<move> All_moves = new List<move>();

    public List<move> P_Moves_LeftUp = new List<move>();    // массивы для направлений
    public List<move> P_Moves_RightUp = new List<move>();

    public List<move> P_Moves_LeftDown = new List<move>();
    public List<move> P_Moves_RightDown = new List<move>();

    public List<move> P_Moves_Up = new List<move>();   
    public List<move> P_Moves_Down = new List<move>();

    public List<move> P_Moves_Left = new List<move>();
    public List<move> P_Moves_Right = new List<move>();

    public GameObject Core_object;
    public bool Can_add = true;

    public void PossibleMoves(int z, int x) //   28 возможных ходов
    {
        Core_object = GameObject.Find("Core");
        Core scriptToAccess = Core_object.GetComponent<Core>();

        int for_z, for_x;
        for_z = z;
        for_x = x;

        for (int i = 1; i < 8; i++)    // Вверх
        {
            move mv = new move();
            mv.x = x;
            mv.z = z + i;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure == 1)
                        {   // добавляем ели цвет не наш
                            P_Moves_Up.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_Up.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // Down
        {
            move mv = new move();
            mv.x = x;
            mv.z = z - i;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure == 1)
                        {   // добавляем ели цвет не наш
                            P_Moves_Down.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_Down.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // Left
        {
            move mv = new move();
            mv.x = x - i;
            mv.z = z;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure == 1)
                        {   // добавляем ели цвет не наш
                            P_Moves_Left.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_Left.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // Right
        {
            move mv = new move();
            mv.x = x + i;
            mv.z = z;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure == 1)
                        {   // добавляем если цвет не наш
                            P_Moves_Right.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_Right.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // RightUp
        {
            move mv = new move();
            mv.x = x + i;
            mv.z = z + i;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure == 1)
                        {   // добавляем ели цвет не наш
                            P_Moves_RightUp.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_RightUp.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // RightDown
        {
            move mv = new move();
            mv.x = x + i;
            mv.z = z - i;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure == 1)
                        {   // добавляем ели цвет не наш
                            P_Moves_RightDown.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_RightDown.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // LeftDown
        {
            move mv = new move();
            mv.x = x - i;
            mv.z = z - i;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure == 1)
                        {   // добавляем ели цвет не наш
                            P_Moves_LeftDown.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_LeftDown.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // LefUp
        {
            move mv = new move();
            mv.x = x - i;
            mv.z = z + i;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure == 1)
                        {   // добавляем ели цвет не наш
                            P_Moves_LeftUp.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_LeftUp.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;


        for (int i = 0; i < P_Moves_LeftUp.Count; i++)
        {
            All_moves.Add(P_Moves_LeftUp[i]);
        }

        for (int i = 0; i < P_Moves_LeftDown.Count; i++)
        {
            All_moves.Add(P_Moves_LeftDown[i]);
        }

        for (int i = 0; i < P_Moves_RightDown.Count; i++)
        {
            All_moves.Add(P_Moves_RightDown[i]);
        }

        for (int i = 0; i < P_Moves_RightUp.Count; i++)
        {
            All_moves.Add(P_Moves_RightUp[i]);
        }

        for (int i = 0; i < P_Moves_Up.Count; i++)
        {
            All_moves.Add(P_Moves_Up[i]);
        }

        for (int i = 0; i < P_Moves_Down.Count; i++)
        {
            All_moves.Add(P_Moves_Down[i]);
        }

        for (int i = 0; i < P_Moves_Left.Count; i++)
        {
            All_moves.Add(P_Moves_Left[i]);
        }

        for (int i = 0; i < P_Moves_Right.Count; i++)
        {
            All_moves.Add(P_Moves_Right[i]);
        }

        for (int i = 0; i < All_moves.Count; i++)
        {
            All_moves[i].name = "queen";
            All_moves[i].started_z = for_z;
            All_moves[i].started_x = for_x;
        }
    }
}
Скрипт ладьи
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class rook : figure
{    //ладья
    public string name = "rook";

    public List<move> All_moves = new List<move>();

    public List<move> P_Moves_Up = new List<move>();    // массивы для направлений
    public List<move> P_Moves_Down = new List<move>();

    public List<move> P_Moves_Left = new List<move>();
    public List<move> P_Moves_Right = new List<move>();

    public GameObject Core_object;
    public bool Can_add = true;

    public void PossibleMoves(int z, int x) //   28 возможных ходов
    {
        Core_object = GameObject.Find("Core");
        Core scriptToAccess = Core_object.GetComponent<Core>();
        int for_z;
        int for_x;

        for_z = z;
        for_x = x;

        int myColor = 0;

        for (int i = 1; i < 8; i++)    // Вверх
        {
            move mv = new move();
            mv.x = x;
            mv.z = z + i;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != myColor & scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].figure_name != "empty")
                        {   // добавляем ели цвет не наш
                            P_Moves_Up.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_Up.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // Вниз
        {
            move mv = new move();
            mv.x = x;
            mv.z = z - i;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != myColor & scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].figure_name != "empty")
                        {   // добавляем если цвет не наш
                            P_Moves_Down.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_Down.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // Влево
        {
            move mv = new move();
            mv.x = x - i;
            mv.z = z;
            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != myColor & scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].figure_name != "empty")
                        {   // добавляем ели цвет не наш
                            P_Moves_Left.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_Left.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // Вправо
        {
            move mv = new move();
            mv.x = x + i;
            mv.z = z;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != myColor & scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].figure_name != "empty")
                        {   // добавляем если цвет не наш
                            P_Moves_Right.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_Right.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 0; i < P_Moves_Up.Count; i++)
        {
            All_moves.Add(P_Moves_Up[i]);
        }
        for (int i = 0; i < P_Moves_Down.Count; i++)
        {
            All_moves.Add(P_Moves_Down[i]);
        }
        for (int i = 0; i < P_Moves_Left.Count; i++)
        {
            All_moves.Add(P_Moves_Left[i]);
        }
        for (int i = 0; i < P_Moves_Right.Count; i++)
        {
            All_moves.Add(P_Moves_Right[i]);
        }

        for (int i = 0; i < All_moves.Count; i++)
        {
            All_moves[i].started_x = for_x;
            All_moves[i].started_z = for_z;
            //Debug.Log("***********");
            //Debug.Log(All_moves[i].started_z);
            //Debug.Log(All_moves[i].started_x);
            //Debug.Log("***********");
        }
    }
}
Скрипт слона
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class bishop : figure
{
    public string name = "bishop";
    public GameObject Core_object;

    public bool Can_add = true;

    public List<move> All_moves = new List<move>();

    public List<move> P_Moves_LeftUp = new List<move>();    // массивы для направлений
    public List<move> P_Moves_RightUp = new List<move>();
    public List<move> P_Moves_LeftDown = new List<move>();
    public List<move> P_Moves_RightDown = new List<move>();

    //возможные ходы для bishop
    public void PossibleMoves(int z, int x) //   28 возможных ходов
    {
        Core_object = GameObject.Find("Core");
        Core scriptToAccess = Core_object.GetComponent<Core>();

        int myColor = 0;
      
        int for_z;
        int for_x;

        for_z = z;
        for_x = x;

        for (int i = 1; i < 8; i++)    // RightUp
        {
            move mv = new move();
            mv.x = x + i;
            mv.z = z + i;

            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // строчка для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != myColor)
                        {   // добавляем если цвет не наш
                            P_Moves_RightUp.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_RightUp.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // RightDown
        {
            move mv = new move();
            mv.x = x + i;
            mv.z = z - i;
            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    //для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != myColor)
                        {   // добавляем ели цвет не наш
                            P_Moves_RightDown.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_RightDown.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // LeftDown
        {
            move mv = new move();
            mv.x = x - i;
            mv.z = z - i;
            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    //для ограничения хода в границах 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != myColor)
                        {   // добавляем ели цвет не наш
                            P_Moves_LeftDown.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_LeftDown.Add(mv);
                }
            }
        }

        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 1; i < 8; i++)    // LefUp
        {
            move mv = new move();
            mv.x = x - i;
            mv.z = z + i;
            if (mv.x >= 0 & mv.x < 8 & mv.z >= 0 & mv.z < 8)    // 8x8 
            {
                if (Can_add)
                {
                    if (scriptToAccess.board[mv.z, mv.x].figure_name != "empty")
                    {
                        Can_add = false;
                        if (scriptToAccess.board[scriptToAccess.second_z, scriptToAccess.second_x].colors_of_figure != myColor)
                        {   // добавляем ели цвет не наш
                            P_Moves_LeftUp.Add(mv);
                        }
                    }
                }

                if (Can_add)
                {
                    P_Moves_LeftUp.Add(mv);
                }
            }
        }
       
        z = for_z;
        x = for_x;

        Can_add = true;

        for (int i = 0; i < P_Moves_LeftUp.Count; i++)
        {
            All_moves.Add(P_Moves_LeftUp[i]);
        }

        z = for_z;
        x = for_x;

        for (int i = 0; i < P_Moves_LeftDown.Count; i++)
        {
            All_moves.Add(P_Moves_LeftDown[i]);
        }

        z = for_z;
        x = for_x;

        for (int i = 0; i < P_Moves_RightDown.Count; i++)
        {
            All_moves.Add(P_Moves_RightDown[i]);
        }

        z = for_z;
        x = for_x;

        for (int i = 0; i < P_Moves_RightUp.Count; i++)
        {
            All_moves.Add(P_Moves_RightUp[i]);
        }

        z = for_z;
        x = for_x;
        
        for (int i = 0; i < All_moves.Count; i++)
        {
            All_moves[i].name = "bishop";
            All_moves[i].started_z = for_z;
            All_moves[i].started_x = for_x;

        }
    }
}
Скрипт выбранной пользователем фигуры
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
/// <summary>
/// Выделение нужных фигур пользователем (граф. часть)
/// </summary>
public class Active : MonoBehaviour
{
    public Renderer rend;
    public Material default_mat;
    public Material mat;
    public Material second_mat;
    public GameObject Core_object;
    private int first_number;
    private int second_number;

    private bool first_active;
    private bool second_active;

    /// <summary>
    /// единожды срабатывает функция в начале
    /// </summary>
    void Awake()
    {
        Core_object = GameObject.Find("Core");
        default_mat = rend.material;

        first_number = (int)this.transform.position.z;
        second_number = (int)this.transform.position.x;
    }
    /// <summary>
    /// смена материала
    /// </summary>
    void LateUpdate()
    {

        Core scriptToAccess = Core_object.GetComponent<Core>();

        for (int i = 0; i < 8; i++)
        {
            for (int j = 0; j < 8; j++)
            {

                if (scriptToAccess.board[first_number, second_number].active == false)
                {
                    rend.material = default_mat;
                }
            }
        }
        
    }
    /// <summary>
    /// событие на нажатие клавиши
    /// </summary>
    void OnMouseUp()        // будет работать только если мы белые
    {
        Core scriptToAccess = Core_object.GetComponent<Core>();

        for (int i = 0; i < 8; i++)
        {
            for (int j = 0; j < 8; j++)
            {

                if (scriptToAccess.board[first_number, second_number].active == false)
                {
                    rend.material = default_mat;
                }
            }
        }
 


        if (scriptToAccess.State == 0)
        {
            if (scriptToAccess.board[first_number, second_number].colors_of_figure == 0)
            {


                if (!first_active)  // если не выбрана первая фигура, то мы вибираем ее
                {
           
                    if (scriptToAccess.board[first_number, second_number].figure_name != "empty")   // пустая фигура не может быть выделена для движения
                    {
                        scriptToAccess.ActivateFigure(this.transform.position.z, this.transform.position.x);
                        scriptToAccess.z = (int)this.transform.position.z;
                        scriptToAccess.x = (int)this.transform.position.x;
                        first_active = true;
                        scriptToAccess.CheckFirstActive();
                        Debug.Log("activated figure is");
                        Debug.Log(scriptToAccess.board[scriptToAccess.z, scriptToAccess.x]);
                        Changing_First_Materials();

                      //  scriptToAccess.DebuggingStats(first_number, second_number);

                    }
                }
            }

            // если фигура пустая то выбираем ее как второе значение, если нет выбираем как первое и если цвет черный то как 2ую выбираем
            if (scriptToAccess.FirstActiveted)
            {
                if (scriptToAccess.board[first_number, second_number].figure_name == "empty")
                {
                    scriptToAccess.SecondActivateFigure(this.transform.position.z, this.transform.position.x);
                    scriptToAccess.second_z = (int)this.transform.position.z;
                    scriptToAccess.second_x = (int)this.transform.position.x;
                    scriptToAccess.isMoveCanBe(scriptToAccess.board[scriptToAccess.z, scriptToAccess.x].figure_name, scriptToAccess.board[first_number, second_number].colors_of_figure);
                    second_active = true;

                    
                }

                else
                {
                    if (scriptToAccess.board[first_number, second_number].colors_of_figure == 1)    // Надо вызывать атаку
                    {
                        scriptToAccess.SecondActivateFigure(this.transform.position.z, this.transform.position.x);
                        scriptToAccess.second_z = (int)this.transform.position.z;
                        scriptToAccess.second_x = (int)this.transform.position.x;
                        scriptToAccess.isMoveCanBe(scriptToAccess.board[scriptToAccess.z, scriptToAccess.x].figure_name, scriptToAccess.board[first_number, second_number].colors_of_figure);
                        second_active = true;
                        
                    }
                    else
                    {

                        scriptToAccess.ActivateFigure(this.transform.position.z, this.transform.position.x);
                        Changing_First_Materials();
                        scriptToAccess.z = (int)this.transform.position.z;
                        scriptToAccess.x = (int)this.transform.position.x;
                        first_active = true;

                    }
                }
            }   // color
            else
            {   // пишем атаку
                


            }

        }   // state

    }

    /// <summary>
    /// меняет материалы
    /// </summary>
    void Changing_First_Materials()
    {
        Core scriptToAccess = Core_object.GetComponent<Core>();
        for (int i = 0; i < 8; i++)
        {
            for (int j = 0; j < 8; j++)
            {

                rend.material = default_mat;

            }
        }


        if (scriptToAccess.board[first_number, second_number].active)
        {
            this.rend.material = mat;
        }

    }
     
}
