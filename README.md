package Clases;


import static Ventanas.VentanaPrincipal.jPanel1;
import static Ventanas.VentanaPrincipal.R_repaint;
import static Ventanas.VentanaPrincipal.ingresarNodoOrigen;
import java.awt.Color;
import javax.swing.JOptionPane;

/**
 *
 * @author fredy-18
 */
public class Algoritmo_Dijkstra {
   private  Arboles arboles;
   private int subTope;
   private Nodo auxi=null;
   private int auxAumulado; // es un acumulado auxiliar
   private int subAcomulado;
   private Nodo nodo[]; 
   private int tope;
   private int permanente;     
   private int nodoFin; 
   
   
    public Algoritmo_Dijkstra (Arboles arboles, int tope,int permanente, int nodoFin){
        this.arboles = arboles;        
        this.tope = tope;
        this.nodo= new Nodo[tope]; 
        this.permanente = permanente;
        this.nodoFin = nodoFin;
        
    }

    public int getAcumulado(){
        return nodo[nodoFin].getAcumulado(); 
    }
        
    public void dijkstra(){ 
         for (int i = 0; i < tope; i++)  // creamos el vector nodo del tamaño de tope el cual tiene el numero de nodo pintados 
                    nodo[i]= new Nodo(); 
         
        if(permanente != nodoFin){
             jPanel1.paint(jPanel1.getGraphics());
             R_repaint(tope, arboles);   
             Pintar.clickSobreNodo(jPanel1.getGraphics(), arboles.getCordeX(permanente), arboles.getCordeY(permanente), null,Color.GREEN); // pinta de color GREEN los nodos
            
        
            nodo[permanente].setVisitado(true);        
            nodo[permanente].setNombre(permanente);       
            
            do{            
              subAcomulado=0;
              auxAumulado = 2000000000; // lo igualamos a esta cifra ya q el acomulado de los nodos, supuestamente  nunca sera mayor 
              nodo[permanente].setEtiqueta(true); 
              for (int j = 0; j < tope; j++) {
                  if(arboles.getmAdyacencia(j, permanente)==1){
                        subAcomulado= nodo[permanente].getAcumulado()+arboles.getmCoeficiente(j, permanente);                                 
                        if(subAcomulado <= nodo[j].getAcumulado() && nodo[j].isVisitado()==true && nodo[j].isEtiqueta()== false){
                            nodo[j].setAcumulado(subAcomulado);
                            nodo[j].setVisitado(true);
                            nodo[j].setNombre(j);
                            nodo[j].setPredecesor(nodo[permanente]);
                        }
                        else if( nodo[j].isVisitado()==false){
                            nodo[j].setAcumulado(subAcomulado);
                            nodo[j].setVisitado(true);
                            nodo[j].setNombre(j);
                            nodo[j].setPredecesor(nodo[permanente]); 
                       }
                 }
              }
              for (int i = 0; i <tope; i++) { // buscamos cual de los nodos visitado tiene el acomulado menor par escogerlo como permanente 
                if(nodo[i].isVisitado()== true && nodo[i].isEtiqueta()== false){
                   if(nodo[i].getAcumulado()<=auxAumulado){
                       permanente= nodo[i].getNombre();
                       auxAumulado= nodo[i].getAcumulado();
                   }
                }               
             }
            subTope++;                
          }while(subTope<tope+1);          
          auxi= nodo[nodoFin]; 
           if(auxi.getPredecesor() == null )
             JOptionPane.showMessageDialog(null,"No se Pudo LLegar Al Nodo "+nodoFin);          
          while(auxi.getPredecesor() != null){           
              Pintar.pintarCamino(jPanel1.getGraphics(), arboles.getCordeX(auxi.getNombre()), arboles.getCordeY(auxi.getNombre()), arboles.getCordeX(auxi.getPredecesor().getNombre()), arboles.getCordeY(auxi.getPredecesor().getNombre()),Color.GREEN);
              Pintar.clickSobreNodo(jPanel1.getGraphics(), arboles.getCordeX(auxi.getNombre()), arboles.getCordeY(auxi.getNombre()), null,Color.GREEN);
             auxi=auxi.getPredecesor();              
          }  
         Pintar.clickSobreNodo(jPanel1.getGraphics(), arboles.getCordeX(nodoFin), arboles.getCordeY(nodoFin), null,Color.GREEN);     
       }
       else Pintar.clickSobreNodo(jPanel1.getGraphics(), arboles.getCordeX(nodoFin), arboles.getCordeY(nodoFin), null,Color.GREEN);    
    }
    
    
 
}
package Clases;

import static Ventanas.VentanaPrincipal.R_repaint;
import static Ventanas.VentanaPrincipal.ingresarNodoOrigen;
import static Ventanas.VentanaPrincipal.jPanel1;
import java.awt.Color;

/**
 *
 * @author fredy-18
 */
public class Algoritmo_Prim {
    
    private int cumulado;
   private int aristaMenor;
   private int  fin;
   private boolean estaNodo=false;
   private boolean aumentaTamano;
   private int nodoApuntado;  
   private int nodoApuntador;
   private int tamano;
   private int arsitaMayor;
   private  Arboles arboles;
   private int tope;
   private  int  nodoOrigen;
   
   
   
   public Algoritmo_Prim(Arboles arbol , int top ,int aristaMayor ){
       this.cumulado = 0;
       this.aristaMenor = 0;
       this.fin = 0;
       this.estaNodo=false;
       this.aumentaTamano = false;
       this.nodoApuntado = 0;  
       this.nodoApuntador = 0;
       this.tamano = 1;
       this. arsitaMayor=aristaMayor;
       this.arboles = arbol;
       this.tope = top;
   }

    public int getCumulado() {
        return cumulado;
    }
  
   
  public void prim(){
      this.nodoOrigen= ingresarNodoOrigen("Ingrese Nodo Origen..","nodo Origen No existe",tope);
       jPanel1.paint(jPanel1.getGraphics());
       R_repaint(tope,arboles);
       arboles.crearEnArbol(tope);
       arboles.setEnArbol(0, nodoOrigen);
       //algoritmo de Prim ---->>
       do{
           this.aristaMenor = this.arsitaMayor;
           this.fin=2;
            for (int j = 0; j < tamano; j++) {
                for (int k = 0; k < tope; k++){
                    if(arboles.getmAdyacencia(k, arboles.getEnArbol(j))==1){
                        for (int h = 0; h < tamano; h++) {
                             if(arboles.getEnArbol(h)==k ){
                                 this.estaNodo=true; 
                                 break;
                             }
                        }
                        if(estaNodo==false){
                            if(arboles.getmCoeficiente(k, arboles.getEnArbol(j))<=aristaMenor && arboles.getmCoeficiente(k, arboles.getEnArbol(j))>0 ){
                                 aristaMenor=arboles.getmCoeficiente(k, arboles.getEnArbol(j));
                                 this.nodoApuntado=k;
                                 this.aumentaTamano=true;
                                 this.nodoApuntador=arboles.getEnArbol(j);
                                 this.fin=1;
                            }
                        }
                        this.estaNodo=false;
                    }
                }
            }//fin  for (int j = 0; j < tamano; j++)              
         if(aumentaTamano==true){
                    Pintar.pintarCamino(jPanel1.getGraphics(),arboles.getCordeX(nodoApuntador), arboles.getCordeY(nodoApuntador),arboles.getCordeX(nodoApuntado), arboles.getCordeY(nodoApuntado),Color.red); 
                    Pintar.clickSobreNodo(jPanel1.getGraphics(),arboles.getCordeX(nodoApuntador), arboles.getCordeY(nodoApuntador), null,Color. red);
                    Pintar.clickSobreNodo(jPanel1.getGraphics(),arboles.getCordeX(nodoApuntado), arboles.getCordeY(nodoApuntado), null, Color.red);                                  
                    arboles.setEnArbol(tamano, nodoApuntado);
                    this.tamano++;
                    this.aumentaTamano=false;
                    this.cumulado += this.aristaMenor;
         }
        }while(fin<2);
   }
    
}
package Clases;

/**
 *
 * @author fredy_000
 */
public class Arboles {
   private int mCoeficiente[][] = new int [51][51];
   private int mAdyacencia [][] = new int [51][51];
   private int cordeX [] = new int [51];
   private int cordeY [] = new int [51];
   private int nombre [] = new int [51];
   private int enArbol [];
   
   
   
   public Arboles(){
        
    }

    public int getmCoeficiente(int i, int j ) {
        return mCoeficiente[i][j];
    }

    public int getmAdyacencia(int i,int j) {
        return mAdyacencia[i][j];
    }

    public int getCordeX(int i) {
        return cordeX[i];
    }

    public int getCordeY(int i) {
        return cordeY[i];
    }

    public int getNombre(int i) {
        return nombre[i];
    }

    public int getEnArbol(int i) {
        return enArbol[i];
    }

    public void setmCoeficiente(int i,int j ,int mCoeficiente) {
        this.mCoeficiente[i][j] = mCoeficiente;
    }

    public void setmAdyacencia(int i,int j ,int mAdyacencia) {
        this.mAdyacencia[i][j] = mAdyacencia;
    }

    public void setCordeX(int i,int cordeX) {
        this.cordeX[i] = cordeX;
    }

    public void setCordeY(int i, int cordeY) {
        this.cordeY[i] = cordeY;
    }

    public void setNombre(int i,int nombre) {
        this.nombre[i] = nombre;
    }

    public void setEnArbol(int i,int enArbol) {
        this.enArbol[i] = enArbol;
    }
    public void crearEnArbol(int i){
       enArbol = new int [i]; 
    }  
    
}
package Clases;

/**
 *
 * @author fredy-18
 */
public class Nodo {    
   private  int nombre ;
   private boolean visitado ;
   private boolean etiqueta;
   private int acumulado; // lleva el acoulado de cada nodo
   private Nodo Predecesor;
   
   public Nodo(){
       this.nombre =-1;
       this.visitado = false;
       this.etiqueta = false;
       this.Predecesor  = null;
       this.acumulado =0;       

   }

     public int getNombre() {
        return nombre;
    }

    public boolean isVisitado() {
        return visitado;
    }

    public boolean isEtiqueta() {
        return etiqueta;
    }

    public int getAcumulado() {
        return acumulado;
    }

    public Nodo getPredecesor() {
        return Predecesor;
    }

    public void setNombre(int nombre) {
        this.nombre = nombre;
    }

    public void setVisitado(boolean visitado) {
        this.visitado = visitado;
    }

    public void setEtiqueta(boolean etiqueta) {
        this.etiqueta = etiqueta;
    }

    public void setAcumulado(int acomulado) {
        this.acumulado = acomulado;
    }

    public void setPredecesor(Nodo Predecesor) {
        this.Predecesor = Predecesor;
    }

    
}
package Principal;

import Ventanas.VentanaArista;
import Ventanas.VentanaPrincipal;




/**
 *
 * @author fredy_000
 */
public class Principal {
    public static void main(String[] args) {
        new VentanaPrincipal().setVisible(true);
 }
}
