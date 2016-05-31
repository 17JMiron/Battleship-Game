import java.util.Scanner;
import java.util.Random;
import java.util.ArrayList;



public class battleShipGame {

    public static void main(String[] args) {
      
        int tries = 0,
        totalHits = 0;
        
        int[][] ships = new int[3][2];
        int[][] board = new int[5][5];
        int[] fire =  new int[2];
 
        setupBoard(board);
        setupShips(ships);
        
        System.out.println();
        
        do{
            showBoard(board);
            fire(fire);
            tries++;
            
            if(hit(fire,ships)){
            
                totalHits++;
            }                
            else
                mark(fire,ships,tries);
            
            updateboard(fire,ships,board);
            

        }while(totalHits!=3);
        
        System.out.println("\n\n\nYou Win! You hit 3 ships in "+tries+" tries");
        showBoard(board);
    }
    
    public static void setupBoard(int[][] board){
        for(int row=0 ; row < 5 ; row++ )
            for(int column=0 ; column < 5 ; column++ )
                board[row][column]=-1;
    }
    
    public static void showBoard(int[][] board){
        System.out.println("\t1 \t2 \t3 \t4 \t5");
        System.out.println();
        
        for(int row=0 ; row < 5 ; row++ ){
            System.out.print((row+1)+"");
            for(int column=0 ; column < 5 ; column++ ){
                if(board[row][column]==-1){
                    System.out.print("\t"+"~");
                }else if(board[row][column]==0){
                    System.out.print("\t"+"O");
                }else if(board[row][column]==1){
                    System.out.print("\t"+"X");
                }
                
            }
            System.out.println();
        }

    }

    public static void setupShips(int[][] ships){
        Random num = new Random();
        
        for(int ship=0 ; ship < 3 ; ship++){
            ships[ship][0]=num.nextInt(5);
            ships[ship][1]=num.nextInt(5);
            
            for(int previous=0 ; previous < ship ; previous++){
                if( (ships[ship][0] == ships[previous][0])&&(ships[ship][1] == ships[previous][1]) )
                    do{
                        ships[ship][0]=num.nextInt(5);
                        ships[ship][1]=num.nextInt(5);
                    }while( (ships[ship][0] == ships[previous][0])&&(ships[ship][1] == ships[previous][1]) );
            }
            
        }
    }

    public static void fire(int[] fire){
        Scanner input = new Scanner(System.in);
        
        System.out.print("Guess a Row: ");
        fire[0] = input.nextInt();
        fire[0]--;
        
        System.out.print("Guess a Column: ");
        fire[1] = input.nextInt();
        fire[1]--;
        
    }
    
    public static boolean hit(int[] fire, int[][] ships){
        
        for(int ship=0 ; ship<ships.length ; ship++){
            if( fire[0]==ships[ship][0] && fire[1]==ships[ship][1]){
                System.out.println("HIT!!! You found a ship located in (" + (fire[0]+1) + "," + (fire[1]+1) + ")\n");
                return true;
            }
        }
        return false;
    }

    public static void mark(int[] fire, int[][] ships, int attempt){
        int r = 0,
         c = 0;
        
        for(int l=0 ; l < ships.length ; l++){
            if(ships[l][0]==fire[0])
                r++;
            if(ships[l][1]==fire[1])
                c++;
        }
        

    }

    public static void updateboard(int[] fire, int[][] ships, int[][] board){
        if(hit(fire,ships))
            board[fire[0]][fire[1]]=1;
        else
            board[fire[0]][fire[1]]=0;
    }
}
