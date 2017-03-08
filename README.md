# OOPharkkis

import java.util.ArrayList;
import java.text.SimpleDateFormat;
import java.util.Calendar;

/*Luo osakkeiden hallintaohjelma.
 */

public class T4 {
	public static void main(String[] args){
		Osake Outokumpu = new Osake("Outokumpu",1.33,1300);
		OsakeSalkku mun = new OsakeSalkku("mun",100000.0);
		mun.lisaaOsake(Outokumpu);
		Outokumpu.LisaaArvo(13);
		Outokumpu.LisaaArvo(12);
		Outokumpu.muutaMaara(mun, 120);
		Outokumpu.LisaaArvo(11);
		Osake Nokia = new Osake("Nokia",2.66,1500);
		mun.lisaaOsake(Nokia);
		Nokia.LisaaArvo(2.69);
		Nokia.LisaaArvo(2.72);
		ArrayList<historiaT> historia = Outokumpu.annaHistoria();
		for(int i =0;i<historia.size();i++){
			System.out.println(historia.get(i));
		}
		System.out.println("-----");
		mun.osakeListaus();
	}
}
class OsakeSalkku{
	private String nimi;
	private ArrayList<Osake> osakkeet;
	private int maara;
	private double budjetti;
	
	
	public OsakeSalkku(String nimi, double budjetti){
		osakkeet = new ArrayList<Osake>();
		this.budjetti = budjetti;
		this.nimi = nimi;
	}
	public ArrayList<Osake> getSalkku(){
		return osakkeet;
	}
	public void lisaaOsake(Osake osake){
		osakkeet.add(osake);
		budjetti-=osake.getHinta()*osake.getMaara();
	}
	public double annaBudjetti(){
		return budjetti;
	}
	public void muutaBudjetti(double budjetti){
		this.budjetti = budjetti;
	}
	public void osakeListaus(){
		System.out.println("Budjettisi on: " +budjetti);
		System.out.println("Osakkeesi:");
		for(int i=0;i<osakkeet.size();i++){
			System.out.println(osakkeet.get(i).toString());
			
		}
	}
}
class Osake{
	private String nimi;
	private ArrayList<historiaT> arvo;
	private double ostohinta;
	private int maara;
	
	public Osake(String nimi,double ostohinta, int maara){
		this.nimi=nimi;
		this.maara = maara;
		this.ostohinta = ostohinta;
		arvo = new ArrayList<historiaT>();
		arvo.add(Time.getTime(),ostohinta);
	}
	public void LisaaArvo(double i){
		historiaT t = new historiaT(Time.getTime(),i, maara);
		arvo.add(t);
	}
	public ArrayList<historiaT> annaHistoria(){
		return arvo;
	}
	public void muutaMaara(OsakeSalkku m, int maara){
		historiaT n = (historiaT) arvo.get(arvo.size()-1);
		if(this.maara<maara){
			m.muutaBudjetti(m.annaBudjetti()-n.annaArvo());
		}
		if(this.maara>maara){
			m.muutaBudjetti(m.annaBudjetti()+n.annaArvo());

		}
		this.maara=maara;
		
	}
	public int getMaara(){
		return maara;
	}
	public double getHinta(){
		historiaT m = (historiaT) arvo.get(arvo.size()-1);
		return m.annaArvo();
	}
	public String toString(){
		historiaT m = (historiaT) arvo.get(arvo.size()-1);
		return nimi + " "+maara+"kpl "+m.annaArvo() +" \n Yhteensä:"+(int) (maara*m.annaArvo())+"€";
	}
}
/**Sisältää historiatietoihin ajan ja hinnan.
 * 
 * @author Henri
 *
 */
class historiaT{
	private String aika;
	private double arvo;
	private int maara;
	
	public historiaT(String aika,double arvo, int maara){
		this.aika=aika;
		this.arvo=arvo;
		this.maara = maara;
	}
	public double annaArvo(){
		return arvo;
	}
	public String toString(){
		return aika+" - "+arvo+"€ "+" "+maara+"kpl \n Yhteensä:"+maara * arvo;
	}
}

class Time {

    public static String getTime() {
        Calendar cal = Calendar.getInstance();
        SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");
        System.out.println( sdf.format(cal.getTime()) );
        return (String) sdf.format(cal.getTime());
    }
}
