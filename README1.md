# Calibration_3alpha_FEBEX
Script for read, modify and export data taken from MBS-FEBEX.

#include <TFile.h>
#include <TString.h>
#include <TH1.h>
#include <TH2.h>
#include <TF1.h>
#include <TGraphErrors.h>
#include <TTree.h>
#include <TLegend.h>
#include <TRandom.h>
#include <TCanvas.h>
#include <TGraph.h>
#include <TStyle.h>
#include <TMarker.h>
#include <TKey.h>
#include <TLine.h>
#include <TPad.h>
#include <TAttMarker.h>
#include <Riostream.h>
#include "stdio.h"
#include "iostream"
#include "fstream"
#include "cstdlib"
#include "cstring"
#include "stdlib.h"

// PIDAR_3alpha.root/Analysis;1/Histograms;1/FPGA;1/FPGA Energy(hitlist) SFP: 1 FEBEX: 0 CHAN: 0;1   direction
// FPGA Energy(hitlist) SFP: 1 FEBEX: 0 CHAN: 8;1            son 8,14,15,11,12.
// root -l Calibration_3alpha.C\(\"PIDAR_3alpha.root\",\"Calibration_3alpha.root\"\)        CORRER ARCHIVO DESDE CONSOLA

//*******************************************************************************************************************
void Calibration_3alpha(const TString& inputfile="", const TString& outputfile="Calibration_3alpha.root")
{

	//const TString& filename="Analysis;1/Histograms;1/FPGA;1/FPGA Energy(hitlist) SFP:  1 FEBEX:  0 CHAN:  0;1";
	TFile *finput = new TFile(inputfile.Data(),"read");
	TFile *output = new TFile(outputfile.Data(),"recreate");

	//	finput->ls();
	
	  //      TH1F *h = (TH1F*)finput->Get(filename.Data());
	//   Aqui leo los histogramas de febex y los renombro
	TDirectory *dir=finput->GetDirectory("Analysis;1/Histograms;1/FPGA;1");
	dir->ls();        								// MUESTRA LA DIRECCION DE LOS ARCHIVOS
	TH1F *h1 = (TH1F*)dir->Get("FPGA Energy(hitlist) SFP:  1 FEBEX:  0 CHAN:  8;1");
	TH1F *h2 = (TH1F*)dir->Get("FPGA Energy(hitlist) SFP:  1 FEBEX:  0 CHAN: 14;1");
	TH1F *h3 = (TH1F*)dir->Get("FPGA Energy(hitlist) SFP:  1 FEBEX:  0 CHAN: 15;1");
	TH1F *h4 = (TH1F*)dir->Get("FPGA Energy(hitlist) SFP:  1 FEBEX:  0 CHAN: 11;1");
	TH1F *h5 = (TH1F*)dir->Get("FPGA Energy(hitlist) SFP:  1 FEBEX:  0 CHAN: 12;1");

// ******************************************************************************************************************************
//     Aqui creo los nuevos histogramas con los datos de los de febex y me queda por canal.
	TH1F *D1chan8= new TH1F("D1chan8","D1chan8",50000,-1000000,1000000);      //aqui hice una re para que me quede energia en ejeX
	//D1chan8->SetLineColor(kMagenta);                               // y le aplico la calibracion tambien,(16384-8688.44)/2.37293
	TH1F *D2chan14= new TH1F("D2chan14","D2chan14",50000,-1000000,1000000);
//	D2chan14->SetLineColor(kRed+1);
//	D2chan14->SetFillColor(kRed);
	TH1F *D3chan15= new TH1F("D3chan15","D3chan15",50000,-1000000,1000000);
	TH1F *D4chan11= new TH1F("D4chan11","D4chan11",50000,-1000000,1000000);
	TH1F *D5chan12= new TH1F("D5chan12","D5chan12",50000,-1000000,1000000);
//********************************************************************************************************************************
// Loop para llenar los histogramas en l eje x con canales.



Int_t nbins_1,n_1,j_1;						
 nbins_1=h1->GetNbinsX();					
 	for (int i_1=1; i_1 < nbins_1; i_1=i_1+1){
		 n_1=h1->GetBinContent(i_1);                 
//		if ( n_1==0 ) continue;
		j_1=50000-i_1;    
		D1chan8->SetBinContent(j_1,n_1);
	}

	D1chan8->Rebin(8);



Int_t nbins_2,n_2,j_2;
 nbins_2=h2->GetNbinsX();
 	for (int i_2=1; i_2 < nbins_2; i_2++){
		 n_2=h2->GetBinContent(i_2);                  
//		if ( n_2==0 ) continue;			
		j_2=50000-i_2;  
		D2chan14->SetBinContent(j_2,n_2);
		
	}

	D2chan14->Rebin(8);



Int_t nbins_3,n_3,j_3;
 nbins_3=h3->GetNbinsX();
 	for (int i_3=1; i_3 < nbins_3; i_3++){
		 n_3=h3->GetBinContent(i_3);                  
//		if ( n_3==0 ) continue;
		j_3=50000-i_3;
		D3chan15->SetBinContent(j_3,n_3);
	}

	D3chan15->Rebin(8);



Int_t nbins_4,n_4,j_4;
 nbins_4=h4->GetNbinsX();
 	for (int i_4=1; i_4 < nbins_4; i_4++){
		 n_4=h4->GetBinContent(i_4);                  
//		if ( n_4==0 ) continue;
		j_4=50000-i_4;
		D4chan11->SetBinContent(j_4,n_4);
	}

	D4chan11->Rebin(8);



Int_t nbins_5,n_5,j_5;
 nbins_5=h5->GetNbinsX();
 	for (int i_5=1; i_5 < nbins_5; i_5++){
		 n_5=h5->GetBinContent(i_5);                 
//		if ( n_5==0 ) continue;
		j_5=50000-i_5;
		D5chan12->SetBinContent(j_5,n_5);
	}

	D5chan12->Rebin(8);


//**************************************************************************************************************
	TH1F *D1_170deg= new TH1F("D1_170deg","D1_170deg",6250,0,6250); 
//		D1_170deg->SetLineColor(kBlue+2); 
//		D1_170deg->SetFillColor(kRed);  
      
	TH1F *D2_160deg= new TH1F("D2_160deg","D2_160deg",6250,0,6250);
//		D2_160deg->SetLineColor(kBlue+2); 
//		D2_160deg->SetFillColor(kRed); 

	TH1F *D3_150deg= new TH1F("D3_150deg","D3_150deg",6250,0,6250);
//		D3_150deg->SetLineColor(kBlue+2); 
//		D3_150deg->SetFillColor(kRed); 

	TH1F *D4_140deg= new TH1F("D4_140deg","D4_140deg",6250,0,6250);
//		D4_140deg->SetLineColor(kBlue+2); 
//		D4_140deg->SetFillColor(kRed); 

	TH1F *D5_130deg= new TH1F("D5_130deg","D5_130deg",6250,0,6250);
//		D5_130deg->SetLineColor(kBlue+2); 
//		D5_130deg->SetFillColor(kRed); 

//*************************************************************************************************************
ofstream off1;
off1.open("D1_3alpha.txt");

Int_t nexbins_1,nex_1,jex_1;						
 nexbins_1=D1chan8->GetNbinsX();					
 	for (int iex_1=1; iex_1 < nexbins_1; iex_1=iex_1+1){
		 nex_1=D1chan8->GetBinContent(iex_1); 
		jex_1 = iex_1;            	
		off1 << jex_1 <<" "<< nex_1 <<endl;   
		D1_170deg->SetBinContent(jex_1,nex_1);   	
	}
off1.close();


ofstream off2;
off2.open("D2_3alpha.txt");

Int_t nexbins_2,nex_2,jex_2;						
 nexbins_2=D2chan14->GetNbinsX();					
 	for (int iex_2=1; iex_2 < nexbins_2; iex_2=iex_2+1){
		 nex_2=D2chan14->GetBinContent(iex_2);             	
		jex_2 = iex_2;
		off2 << jex_2 <<" "<< nex_2 <<endl;  
		D2_160deg->SetBinContent(jex_2,nex_2);      	
	}
off2.close();


ofstream off3;
off3.open("D3_3alpha.txt");

Int_t nexbins_3,nex_3,jex_3;						
 nexbins_3=D3chan15->GetNbinsX();					
 	for (int iex_3=1; iex_3 < nexbins_3; iex_3=iex_3+1){
		 nex_3=D3chan15->GetBinContent(iex_3);             	
		jex_3 = iex_3;
		off3 << jex_3 <<" "<< nex_3 <<endl;
		D3_150deg->SetBinContent(jex_3,nex_3);        	
	}
off3.close();


ofstream off4;
off4.open("D4_3alpha.txt");

Int_t nexbins_4,nex_4,jex_4;						
 nexbins_4=D4chan11->GetNbinsX();					
 	for (int iex_4=1; iex_4 < nexbins_4; iex_4=iex_4+1){
		 nex_4=D4chan11->GetBinContent(iex_4);
		jex_4 = iex_4;             	
		off4 << jex_4 <<" "<< nex_4 <<endl;      	
		D4_140deg->SetBinContent(jex_4,nex_4);  
	}
off4.close();



ofstream off5;
off5.open("D5_3alpha.txt");

Int_t nexbins_5,nex_5,jex_5;						
 nexbins_5=D5chan12->GetNbinsX();					
 	for (int iex_5=1; iex_5 < nexbins_5; iex_5=iex_5+1){
		 nex_5=D5chan12->GetBinContent(iex_5); 
		jex_5 = iex_5;            	
		off5 << jex_5 <<" "<< nex_5 <<endl;
		D5_130deg->SetBinContent(jex_5,nex_5);        	
	}
off5.close();

//****************************************************************************************************************************
//		Aqui dibujo los histogramas en conteos por canal

//	TCanvas *c = new TCanvas("c", "Spectrums3alpha",55,52,700,500);
//	c1->SetGridx();
//	c1->SetGridy();
//	c->cd();
//	D1chan8->Draw();
//	D2chan14->Draw("same");
//	D3chan15->Draw("same");
//	D4chan11->Draw("same");
//	D5chan12->Draw("same");
//	c->Write();


//	TH1F *h1= new TH1F("hgaus","histo from a gaussian",100,-3,3);
//	h1->FillRandom("gaus",10000);
//	h1->Write();
//   		
//	output->cd();
	h1->Write();
	h2->Write();
	h3->Write();
	h4->Write();
	h5->Write();

//	D1chan8->GetXaxis()->SetTitle("Canales");
//	D1chan8->GetYaxis()->SetTitle("Cuentas");

//	D2chan14->GetXaxis()->SetTitle("Canales");
//	D2chan14->GetYaxis()->SetTitle("Cuentas");

//	D3chan15->GetXaxis()->SetTitle("Canales");
//	D3chan15->GetYaxis()->SetTitle("Cuentas");

//	D4chan11->GetXaxis()->SetTitle("Canales");
//	D4chan11->GetYaxis()->SetTitle("Cuentas");

//	D5chan12->GetXaxis()->SetTitle("Canales");
//	D5chan12->GetYaxis()->SetTitle("Cuentas");

	D1chan8->Write();
	D2chan14->Write();
	D3chan15->Write();
	D4chan11->Write();
	D5chan12->Write();

	D1_170deg->Write();
	D2_160deg->Write();
	D3_150deg->Write();
	D4_140deg->Write();
	D5_130deg->Write();
//*************************************************************************************************************************

//output->Write();
/*
TGraph *g= new TGraph();
g->SetPoint(0, 5157, 3.54760e+03);
g->SetPoint(1, 5486, 4.33173e+03);
g->SetPoint(2, 5805, 5.08522e+03);
g->Draw("AL*");
g->Fit("pol1","","",5100,5810);   //Aqui se le da el rango manual
g->GetXaxis()->SetTitle("Energy [keV]");
g->GetYaxis()->SetTitle("Canales");
g->Write();

*/


//**************************************************************************************************************************

//              Ajustes 

//		Gauss detector 1

	TF1 *fg1_D1 = new TF1("fg1_D1","gaus");
	fg1_D1->SetLineColor(kGreen);
	fg1_D1->SetLineWidth(2);
	TF1 *fg2_D1 = new TF1("fg2_D1","gaus");
	fg2_D1->SetLineColor(kGreen);
	fg2_D1->SetLineWidth(2);
	TF1 *fg3_D1 = new TF1("fg3_D1","gaus");
	fg3_D1->SetLineColor(kGreen);
	fg3_D1->SetLineWidth(2);
	D1_170deg->Fit("fg1_D1","", "", 4637, 4656);
	D1_170deg->Fit("fg2_D1","", "", 4736, 4750);
	D1_170deg->Fit("fg3_D1","", "", 4830, 4846);

	float mean1_D1 =  fg1_D1->GetParameter(1);
	float sigma1_D1 = fg1_D1->GetParameter(2);

	float mean2_D1 =  fg2_D1->GetParameter(1);
	float sigma2_D1 = fg2_D1->GetParameter(2);

	float mean3_D1 =  fg3_D1->GetParameter(1);
	float sigma3_D1 = fg3_D1->GetParameter(2);

	cout << "	   " <<   endl;
	cout << "	Chanal Medio1_D1:   " <<  mean1_D1 << endl;
	cout << "       Sigma1_D1:          "        << sigma1_D1 << endl;
	cout << "	   " <<   endl;
	cout << "	Chanal Medio2_D1:   " <<  mean2_D1 << endl;
	cout << "       Sigma2_D1:          "        << sigma2_D1 << endl;	
	cout << "	   " <<   endl;
	cout << "	Chanal Medio3_D1:   " <<  mean3_D1 << endl;
	cout << "       Sigma3_D1:          " << sigma3_D1 << endl;
	cout <<"\n";
	cout <<"==========================================================================\n";
	cout <<"\n";

//		Gauss detector 2

	TF1 *fg1_D2 = new TF1("fg1_D2","gaus");
	fg1_D2->SetLineColor(kGreen);
	fg1_D2->SetLineWidth(2);
	TF1 *fg2_D2 = new TF1("fg2_D2","gaus");
	fg2_D2->SetLineColor(kGreen);
	fg2_D2->SetLineWidth(2);
	TF1 *fg3_D2 = new TF1("fg3_D2","gaus");
	fg3_D2->SetLineColor(kGreen);
	fg3_D2->SetLineWidth(2);
	D2_160deg->Fit("fg1_D2","", "", 4658, 4672);
	D2_160deg->Fit("fg2_D2","", "", 4758, 4770);
	D2_160deg->Fit("fg3_D2","", "", 4854, 4866);

	float mean1_D2 =  fg1_D2->GetParameter(1);
	float sigma1_D2 = fg1_D2->GetParameter(2);

	float mean2_D2 =  fg2_D2->GetParameter(1);
	float sigma2_D2 = fg2_D2->GetParameter(2);

	float mean3_D2 =  fg3_D2->GetParameter(1);
	float sigma3_D2 = fg3_D2->GetParameter(2);

	cout << "	   " <<   endl;
	cout << "	Chanal Medio1_D2:   " <<  mean1_D2 << endl;
	cout << "       Sigma1_D2:          "        << sigma1_D2 << endl;
	cout << "	   " <<   endl;
	cout << "	Chanal Medio2_D2:   " <<  mean2_D2 << endl;
	cout << "       Sigma2_D2:          "        << sigma2_D2 << endl;	
	cout << "	   " <<   endl;
	cout << "	Chanal Medio3_D2:   " <<  mean3_D2 << endl;
	cout << "       Sigma3_D2:          "        << sigma3_D2 << endl;
	cout << "	   " <<   endl;
	cout <<"\n";
	cout <<"==========================================================================\n";
	cout <<"\n";

//		Gauss Detector 3

	TF1 *fg1_D3 = new TF1("fg1_D3","gaus");
	fg1_D3->SetLineColor(kGreen);
	fg1_D3->SetLineWidth(2);
	TF1 *fg2_D3 = new TF1("fg2_D3","gaus");
	fg2_D3->SetLineColor(kGreen);
	fg2_D3->SetLineWidth(2);
	TF1 *fg3_D3 = new TF1("fg3_D3","gaus");
	fg3_D3->SetLineColor(kGreen);
	fg3_D3->SetLineWidth(2);
	D3_150deg->Fit("fg1_D3","", "", 4604, 4620);
	D3_150deg->Fit("fg2_D3","", "", 4702, 4714);
	D3_150deg->Fit("fg3_D3","", "", 4794, 4806);

	float mean1_D3 =  fg1_D3->GetParameter(1);
	float sigma1_D3 = fg1_D3->GetParameter(2);

	float mean2_D3 =  fg2_D3->GetParameter(1);
	float sigma2_D3 = fg2_D3->GetParameter(2);

	float mean3_D3 =  fg3_D3->GetParameter(1);
	float sigma3_D3 = fg3_D3->GetParameter(2);

	cout << "	   " <<   endl;
	cout << "	Chanal Medio1_D3:   "  << mean1_D3 << endl;
	cout << "       Sigma1_D3:          "        << sigma1_D3 << endl;
	cout << "	   " <<   endl;
	cout << "	Chanal Medio2_D3:   "  << mean2_D3 << endl;
	cout << "       Sigma2_D3:          "        << sigma2_D3 << endl;	
	cout << "	   " <<   endl;
	cout << "	Chanal Medio3_D3:   "  << mean3_D3 << endl;
	cout << "	Sigma3_D3:          "        << sigma3_D3 << endl;
	cout << "	   " <<   endl;
	cout <<"\n";
	cout <<"==========================================================================\n";
	cout <<"\n";

//		Gauss Detector 4

	TF1 *fg1_D4 = new TF1("fg1_D4","gaus");
	fg1_D4->SetLineColor(kGreen);
	fg1_D4->SetLineWidth(2);
	TF1 *fg2_D4 = new TF1("fg2_D4","gaus");
	fg2_D4->SetLineColor(kGreen);
	fg2_D4->SetLineWidth(2);
	TF1 *fg3_D4 = new TF1("fg3_D4","gaus");
	fg3_D4->SetLineColor(kGreen);
	fg3_D4->SetLineWidth(2);
	D4_140deg->Fit("fg1_D4","", "", 4660, 4680);
	D4_140deg->Fit("fg2_D4","", "", 4764, 4778);
	D4_140deg->Fit("fg3_D4","", "", 4858, 4872);

	float mean1_D4 =  fg1_D4->GetParameter(1);
	float sigma1_D4 = fg1_D4->GetParameter(2);

	float mean2_D4 =  fg2_D4->GetParameter(1);
	float sigma2_D4 = fg2_D4->GetParameter(2);

	float mean3_D4 =  fg3_D4->GetParameter(1);
	float sigma3_D4 = fg3_D4->GetParameter(2);

	cout << "	   " <<   endl;
	cout << "	Chanal Medio1_D4:   " <<  mean1_D4 << endl;
	cout << "       Sigma1_D4:          "        << sigma1_D4 << endl;
	cout << "	   " <<   endl;
	cout << "	Chanal Medio2_D4:   " <<  mean2_D4 << endl;
	cout << "       Sigma2_D4:          "        << sigma2_D4 << endl;	
	cout << "	   " <<   endl;
	cout << "	Chanal Medio3_D4:   " <<  mean3_D4 << endl;
	cout << "	Sigma3_D4:          "        << sigma3_D4 << endl;
	cout << "	   " <<   endl;
	cout <<"\n";
	cout <<"==========================================================================\n";
	cout <<"\n";
//		Gauss Detector 5

	TF1 *fg1_D5 = new TF1("fg1_D5","gaus");
	fg1_D5->SetLineColor(kGreen);
	fg1_D5->SetLineWidth(2);
	TF1 *fg2_D5 = new TF1("fg2_D5","gaus");
	fg2_D5->SetLineColor(kGreen);
	fg2_D5->SetLineWidth(2);
	TF1 *fg3_D5 = new TF1("fg3_D5","gaus");
	fg3_D5->SetLineColor(kGreen);
	fg3_D5->SetLineWidth(2);
	D5_130deg->Fit("fg1_D5","", "", 4628, 4642);
	D5_130deg->Fit("fg2_D5","", "", 4726, 4738);
	D5_130deg->Fit("fg3_D5","", "", 4820, 4832);

	float mean1_D5 =  fg1_D5->GetParameter(1);
	float sigma1_D5 = fg1_D5->GetParameter(2);

	float mean2_D5 =  fg2_D5->GetParameter(1);
	float sigma2_D5 = fg2_D5->GetParameter(2);

	float mean3_D5 =  fg3_D5->GetParameter(1);
	float sigma3_D5 = fg3_D5->GetParameter(2);

	cout << "	Chanal Medio1_D5:   " <<  mean1_D5 << endl;
	cout << "       Sigma1_D5:          "        << sigma1_D5 << endl;
	cout << "	   " <<   endl;
	cout << "	Chanal Medio2_D5:   " <<  mean2_D5 << endl;
	cout << "       Sigma2_D5:          "        << sigma2_D5 << endl;	
	cout << "	   " <<   endl;
	cout << "	Chanal Medio3_D5:   " <<  mean3_D5 << endl;
	cout << "       Sigma3_D5:          "        << sigma3_D5 << endl;
	cout << "	   " <<   endl;
	cout <<"\n";
	cout <<"==========================================================================\n";
	cout <<"\n";

//**********************************************************************************************************
// 			Linear Regresion Chi-Square Procedure 



//                Linear Fit Detector 1 

	const int n_D1=3;
	Float_t E_1[n_D1] = {5157,5486,5805};					// Energias de la fuente 3-alpha
	Float_t CH_1[n_D1] = {mean1_D1, mean2_D1, mean3_D1};
	//Float_t eE_1[n_D1] = {.8,.7,.6,.5,.4,.4,.5,.6,.7,.8};
	//Float_t eCH_1[n_D1] = {2.14957e-01,2.39885e-01 ,4.75953e-01};

	TCanvas *cl1 = new TCanvas("cl1", "Ajuste Lineal Detector 1",55,52,700,500);
	
	TF1 *fl_D1 = new TF1("fl_D1","pol1");
	TGraph *gr_1 = new TGraph(n_D1,CH_1,E_1);
	
	gr_1->SetLineColor(kRed);
	gr_1->SetLineWidth(2);
	gr_1->SetLineStyle(1);
	gr_1->SetMarkerColor(1);
	gr_1->SetMarkerStyle(20);

//	gr_1->Fit("fl_D1","","",5157,5805);
	gr_1->Fit("fl_D1","","");
	gr_1->Draw("AP");
	gr_1->Write();

//	cl1->Update();
	cl1->cd();
	cl1->Write();

	float p0_D1 = fl_D1->GetParameter(0);
	float p1_D1 = fl_D1->GetParameter(1);
	cout << "        "<< endl;
	cout << "p0_D1:   " << p0_D1 << endl;
	cout << "p1_D1:   " << p1_D1 << endl;
	

//                Linear Fit Detector 2 

	const int n_D2=3;
	Float_t E_2[n_D2] = {5157,5486,5805};					// Energias de la fuente 3-alpha
	Float_t CH_2[n_D2] = {mean1_D2, mean2_D2, mean3_D2};
	//Float_t eE_2[n_D2] = {.8,.7,.6,.5,.4,.4,.5,.6,.7,.8};
	//Float_t eCH_2[n_D2] = {2.14957e-01,2.39885e-01 ,4.75953e-01};

	TCanvas *cl2 = new TCanvas("cl2", "Ajuste Lineal Detector 2",55,52,700,500);
	
	TF1 *fl_D2 = new TF1("fl_D2","pol1");
	TGraph *gr_2 = new TGraph(n_D2,CH_2,E_2);

	gr_2->SetLineColor(kRed);
	gr_2->SetLineWidth(2);
	gr_2->SetLineStyle(1);
	gr_2->SetMarkerColor(1);
	gr_2->SetMarkerStyle(20);

	gr_2->Draw("AP");
	gr_2->Fit("fl_D2","","",mean1_D2,mean3_D2);
	gr_2->Write();

	cl2->cd();
	cl2->Write();

	float p0_D2 = fl_D2->GetParameter(0);
	float p1_D2 = fl_D2->GetParameter(1);
	cout << "        "<< endl;
	cout << "p0_D2:   " << p0_D2 << endl;
	cout << "p1_D2:   " << p1_D2 << endl;

//                Linear Fit Detector 3 

	const int n_D3=3;
	Float_t E_3[n_D3] = {5157,5486,5805};					// Energias de la fuente 3-alpha
	Float_t CH_3[n_D3] = {mean1_D3, mean2_D3, mean3_D3};
	//Float_t eE_3[n_D3] = {.8,.7,.6,.5,.4,.4,.5,.6,.7,.8};
	//Float_t eCH_3[n_D3] = {2.14957e-01,2.39885e-01 ,4.75953e-01};

	TCanvas *cl3 = new TCanvas("cl3", "Ajuste Lineal Detector 3",55,52,700,500);
	
	TF1 *fl_D3 = new TF1("fl_D3","pol1");
	TGraph *gr_3 = new TGraph(n_D3,CH_3,E_3);

	gr_3->SetLineColor(kRed);
	gr_3->SetLineWidth(2);
	gr_3->SetLineStyle(1);
//	gr_3>SetMarkerColor(1);
	gr_3->SetMarkerStyle(20);

	gr_3->Draw("AP");
	gr_3->Fit("fl_D3","","",mean1_D3,mean3_D3);
	gr_3->Write();

	cl3->cd();
	cl3->Write();

	float p0_D3 = fl_D3->GetParameter(0);
	float p1_D3 = fl_D3->GetParameter(1);
	cout << "        "<< endl;
	cout << "p0_D3:   " << p0_D3 << endl;
	cout << "p1_D3:   " << p1_D3 << endl;

//                Linear Fit Detector 4 

	const int n_D4=3;
	Float_t E_4[n_D4] = {5157,5486,5805};					// Energias de la fuente 3-alpha
	Float_t CH_4[n_D4] = {mean1_D4, mean2_D4, mean3_D4};
	//Float_t eE_4[n_D4] = {.8,.7,.6,.5,.4,.4,.5,.6,.7,.8};
	//Float_t eCH_4[n_D4] = {2.14957e-01,2.39885e-01 ,4.75953e-01};

	TCanvas *cl4 = new TCanvas("cl4", "Ajuste Lineal Detector 4",55,52,700,500);
	
	TF1 *fl_D4 = new TF1("fl_D4","pol1");
	TGraph *gr_4 = new TGraph(n_D4,CH_4,E_4);

	gr_4->SetLineColor(kRed);
	gr_4->SetLineWidth(2);
	gr_4->SetLineStyle(1);
//	gr_4>SetMarkerColor(1);
	gr_4->SetMarkerStyle(20);

	gr_4->Draw("AP");
	gr_4->Fit("fl_D4","","",mean1_D4,mean1_D4);
	gr_4->Write();

	cl4->cd();
	cl4->Write();

	float p0_D4 = fl_D4->GetParameter(0);
	float p1_D4 = fl_D4->GetParameter(1);
	cout << "        "<< endl;
	cout << "p0_D4:   " << p0_D4 << endl;
	cout << "p1_D4:   " << p1_D4 << endl;

//                Linear Fit Detector 5 

	const int n_D5=3;
	Float_t E_5[n_D5] = {5157,5486,5805};					// Energias de la fuente 3-alpha
	Float_t CH_5[n_D5] = {mean1_D5, mean2_D5, mean3_D5};
	//Float_t eE_5[n_D5] = {.8,.7,.6,.5,.4,.4,.5,.6,.7,.8};
	//Float_t eCH_5[n_D5] = {2.14957e-01,2.39885e-01 ,4.75953e-01};

	TCanvas *cl5 = new TCanvas("cl5", "Ajuste Lineal Detector 5",55,52,700,500);
	
	TF1 *fl_D5 = new TF1("fl_D5","pol1");
	TGraph *gr_5 = new TGraph(n_D5,CH_5,E_5);

	gr_5->SetLineColor(kRed);
	gr_5->SetLineWidth(2);
	gr_5->SetLineStyle(1);
//	gr_5>SetMarkerColor(1);
	gr_5->SetMarkerStyle(20);

	gr_5->Draw("AP");
	gr_5->Fit("fl_D5","","",mean1_D5,mean3_D5);
	gr_5->Write();

	cl5->cd();
	cl5->Write();

	float p0_D5 = fl_D5->GetParameter(0);
	float p1_D5 = fl_D5->GetParameter(1);
	cout << "        "<< endl;
	cout << "p0_D5:   " << p0_D5 << endl;
	cout << "p1_D5:   " << p1_D5 << endl;


ofstream off_par;
off_par.open("Parametros_LinearFit.dat");
	cout <<  "     "<< endl;
	cout << " recta D1 ->   E = " <<p1_D1 << " Ch + " << p0_D1 << endl;
	cout << " recta D2 ->   E = " <<p1_D2 << " Ch + " << p0_D2 << endl;
	cout << " recta D3 ->   E = " <<p1_D3 << " Ch + " << p0_D3 << endl;
	cout << " recta D4 ->   E = " <<p1_D4 << " Ch + " << p0_D4 << endl;
	cout << " recta D5 ->   E = " <<p1_D5 << " Ch + " << p0_D5 << endl;
	cout <<  "     "<<endl;
	cout <<  "     "<<endl;
	off_par << " recta D1 ->   E = " << p1_D1 << " Ch + " << p0_D1 << endl;
	off_par << " recta D2 ->   E = " << p1_D2 << " Ch + " << p0_D2 << endl;
	off_par << " recta D3 ->   E = " << p1_D3 << " Ch + " << p0_D3 << endl;
	off_par << " recta D4 ->   E = " << p1_D4 << " Ch + " << p0_D4 << endl;
	off_par << " recta D5 ->   E = " << p1_D5 << " Ch + " << p0_D5 << endl;

	off_par <<  "     "<<endl;
	off_par <<  "  Canal medio de los diferentes picos Pu-239 Am-241 Cm-244   "<<endl;
	off_par <<  "     "<<endl;

	off_par << " " << mean1_D1 << "     " << mean2_D1 << "    " << mean3_D1 << endl;
	off_par << " " << mean1_D2 << "     " << mean2_D2 << "    " << mean3_D2 << endl;
	off_par << " " << mean1_D3 << "     " << mean2_D3 << "    " << mean3_D3 << endl;
	off_par << " " << mean1_D4 << "     " << mean2_D4 << "    " << mean3_D4 << endl;
	off_par << " " << mean1_D5 << "     " << mean2_D5 << "    " << mean3_D5 << endl;

	off_par <<  "     "<<endl;
	off_par <<  "     "<<endl;

off_par.close();

//**********************************************************************************************************************
	
	 

//**********************************************************************************************************************
//		Creacion de los histogramas que tendran energia en el eje X
	TH1F *D1_Cal_170deg= new TH1F("D1_Cal_170deg","D1_Cal_170deg",6250,0, 6250);       
//	D1_Cal_170deg->SetLineColor(kBlue+2); 
//	D1_Cal_170deg->SetFillColor(kRed);  
                              
	TH1F *D2_Cal_160deg= new TH1F("D2_Cal_160deg","D2_Cal_160deg",6250,0,6250);
//	D2_Cal_160deg->SetLineColor(kBlue+2); 
//	D2_Cal_160deg->SetFillColor(kRed);

	TH1F *D3_Cal_150deg= new TH1F("D3_Cal_150deg","D3_Cal_150deg",6250,0,6250);
//	D3_Cal_150deg->SetLineColor(kBlue+2); 
//	D3_Cal_150deg->SetFillColor(kRed);

	TH1F *D4_Cal_140deg= new TH1F("D4_Cal_140deg","D4_Cal_140deg",6250,0,6250);
//	D4_Cal_140deg->SetLineColor(kBlue+2); 
//	D4_Cal_140deg->SetFillColor(kRed);

	TH1F *D5_Cal_130deg= new TH1F("D5_Cal_130deg","D5_Cal_130deg",6250,0,6250);
	D5_Cal_130deg->SetLineWidth(2);
//	D5_Cal_130deg->SetLineColor(kBlue+2); 
//	D5_Cal_130deg->SetFillColor(kRed);


	// Loop para llenar los histogramas en l eje x con Energias.
	

Int_t nubins_1,nu_1,ju_1;
 nubins_1=D1_170deg->GetNbinsX();
 	for (int iu_1=1; iu_1 < nubins_1; iu_1++){
		 nu_1=D1chan8->GetBinContent(iu_1);                  // getbincontent devuelve el numero de eventos de ese bin det1
//		if ( nu_1==0 ) continue;
		ju_1=(iu_1-p0_D1)/p1_D1;
		
		D1_Cal_170deg->SetBinContent(ju_1,nu_1);
	}


Int_t nubins_2,nu_2,ju_2;
 nubins_2=D2_160deg->GetNbinsX();
 	for (int iu_2=1; iu_2 < nubins_2; iu_2++){
		 nu_2=D2_160deg->GetBinContent(iu_2);                  // getbincontent devuelve el numero de eventos de ese bin det2
//		if ( nu_2==0 ) continue;
		ju_2=(iu_2-p0_D2)/p1_D2;
		D2_Cal_160deg->SetBinContent(ju_2,nu_2);
	}

Int_t nubins_3,nu_3,ju_3;
 nubins_3=D3_150deg->GetNbinsX();
 	for (int iu_3=1; iu_3 < nubins_3; iu_3++){
		 nu_3=D3_150deg->GetBinContent(iu_3);                  // getbincontent devuelve el numero de eventos de ese bin det3
//		if ( nu_3==0 ) continue;
		ju_3=(iu_3-p0_D3)/p1_D3;
		D3_Cal_150deg->SetBinContent(ju_3,nu_3);
	}

Int_t nubins_4,nu_4,ju_4;
 nubins_4=D4_140deg->GetNbinsX();
 	for (int iu_4=1; iu_4 < nubins_4; iu_4++){
		 nu_4=D4_140deg->GetBinContent(iu_4);                  // getbincontent devuelve el numero de eventos de ese bin det4
//		if ( nu_4==0 ) continue;
//		ju_4=(iu_4-p0_D4)/p1_D4;
		ju_4=(iu_4-p0_D4)/p1_D4;
		D4_Cal_140deg->SetBinContent(ju_4,nu_4);
	}

Int_t nubins_5,nu_5,ju_5;
 nubins_5=D5_130deg->GetNbinsX();
 	for (int iu_5=1; iu_5 < nubins_5; iu_5++){
		 nu_5=D5_130deg->GetBinContent(iu_5);                  // getbincontent devuelve el numero de eventos de ese bin det5
//		if ( nu_5==0 ) continue;
		ju_5=(iu_5-p0_D5)/p1_D5;
		D5_Cal_130deg->SetBinContent(ju_5,nu_5);
	}

	
//*************************************************************************************************************************************	
//			FWHM
/*
	TF1 *fge1_D5 = new TF1("fge1_D5","gaus");
	fge1_D5->SetLineColor(kGreen);
	fge1_D5->SetLineWidth(2);
	TF1 *fge2_D5 = new TF1("fge2_D5","gaus");
	fge2_D5->SetLineColor(kGreen);
	fge2_D5->SetLineWidth(2);
	TF1 *fge3_D5 = new TF1("fge3_D5","gaus");
	fge3_D5->SetLineColor(kGreen);
	fge3_D5->SetLineWidth(2);
	D5chan12->Fit("fge1_D5","", "", 3400, 3540);
	D5chan12->Fit("fge2_D5","", "", 4192, 4300);
	D5chan12->Fit("fge3_D5","", "", 4940, 5040);

	float mean1e_D5 =  fge1_D5->GetParameter(1);
	float sigma1e_D5 = fge1_D5->GetParameter(2);

	float mean2e_D5 =  fge2_D5->GetParameter(1);
	float sigma2e_D5 = fge2_D5->GetParameter(2);

	float mean3e_D5 =  fge3_D5->GetParameter(1);
	float sigma3e_D5 = fge3_D5->GetParameter(2);

	cout << "	Chanal Medio1_D5:   " <<  mean1_D5 << endl;
	cout << "       Sigma1_D5:   "        << sigma1_D5 << endl;

	cout << "	Chanal Medio2_D5:   " <<  mean2_D5 << endl;
	cout << "       Sigma2_D5:   "        << sigma2_D5 << endl;	

	cout << "	Chanal Medio3_D5:   " <<  mean3_D5 << endl;
	cout << "       Sigma3_D5:   "        << sigma3_D5 << endl;

*/
/*	cout << " FWHM Pu-239 Detector 1:    "<< 2.35*sigma1_D1 << endl;
	cout << " FWHM Pu-239 Detector 2:    "<< 2.35*sigma1_D1 << endl;
	cout << " FWHM Pu-239 Detector 3:    "<< 2.35*sigma1_D2 << endl;
	cout << " FWHM Pu-239 Detector 4:    "<< 2.35*sigma1_D3 << endl;
	cout << " FWHM Pu-239 Detector 5:    "<< 2.35*sigma1_D4 << endl;
*/
//	Escribiendo los histogramas en el eje x con Energia

//	D1Energy8->GetXaxis()->SetTitle("Energ#acute{i}a [keV]");
//	D1Energy8->GetYaxis()->SetTitle("Cuentas");

//	D2Energy14->GetXaxis()->SetTitle("Energ#acute{i}a [keV]");
//	D2Energy14->GetYaxis()->SetTitle("Cuentas");

//	D3Energy15->GetXaxis()->SetTitle("Energ#acute{i}a [keV]");
//	D3Energy15->GetYaxis()->SetTitle("Cuentas");

//	D4Energy11->GetXaxis()->SetTitle("Energ#acute{i}a [keV]");
//	D4Energy11->GetYaxis()->SetTitle("Cuentas");

//	D5Energy12->GetXaxis()->SetTitle("Energ#acute{i}a [keV]");
//	D5Energy12->GetYaxis()->SetTitle("Cuentas");


/*
	Float_t c_1,e_1,cuentas_1[2000],energy_1[12500];
	Float_t c_2,e_2,cuentas_2[2000],energy_2[12500];
	Float_t c_3,e_3,cuentas_3[2000],energy_3[12500];
	Float_t c_4,e_4,cuentas_4[2000],energy_4[12500]; 
	Float_t c_5,e_5,cuentas_5[2000],energy_5[12500]; 
	
	Int_t entries_1=0, entries_2=0, entries_3=0, entries_4=0, entries_5=0;	
	
ifstream in_1;
 in_1.open("D1_3alpha.txt"); 
 while (in_1.good()) {  
      in_1 >> e_1 >> c_1;

//		if (in_1.fail()){
//		cerr << "Archivo no encontrado.....\n";
//		exit(1);      
//		}

               energy_1[entries_1] = e_1;
	       cuentas_1[entries_1] = c_1;
		
      	 entries_1++;
       }

   in_1.close();



ifstream in_2;
 in_2.open("D2_3alpha.txt"); 
 while (in_2.good()) {  
      in_2 >> e_2 >> c_2;

		if (in_2.fail()){
		cerr << "Archivo no encontrado.....\n";
		exit(1);      
		}

               energy_2[entries_2] = e_2;
	       cuentas_2[entries_2] = c_2;
		
      	 entries_2++;
       }

   in_2.close();




ifstream in_3;
 in_3.open("D3_3alpha.txt"); 
 while (in_3.good()) {  
      in_3 >> e_3 >> c_3;

		if (in_3.fail()){
		cerr << "Archivo no encontrado.....\n";
		exit(1);      
		}

               energy_3[entries_3] = e_3;
	       cuentas_3[entries_3] = c_3;
		
      	 entries_3++;
       }

   in_3.close();



ifstream in_4;
 in_4.open("D4_3alpha.txt"); 
 while (in_4.good()) {  
      in_4>> e_4 >> c_4;

		if (in_4.fail()){
		cerr << "Archivo no encontrado.....\n";
		exit(1);      
		}

               energy_4[entries_4] = e_4;
	       cuentas_4[entries_4] = c_4;
		
      	 entries_4++;
       }

   in_4.close();



ifstream in_5;
 in_5.open("D5_3alpha.txt"); 
 while (in_5.good()) {  
      in_5 >> e_5 >> c_5;

		if (in_5.fail()){
		cerr << "Archivo no encontrado.....\n";
		exit(1);      
		}

               energy_5[entries_5] = e_5;
	       cuentas_5[entries_5] = c_5;
		
      	 entries_5++;
       }

   in_5.close();

TGraph *g1 = new TGraph (entries_1, energy_1, cuentas_1);
TGraph *g2 = new TGraph (entries_2, energy_2, cuentas_2);
TGraph *g3 = new TGraph (entries_3, energy_3, cuentas_3);
TGraph *g4 = new TGraph (entries_4, energy_4, cuentas_4);
TGraph *g5 = new TGraph (entries_5, energy_5, cuentas_5);

*/

	D1_Cal_170deg->Write();
	D2_Cal_160deg->Write();
	D3_Cal_150deg->Write();
	D4_Cal_140deg->Write();
	D5_Cal_130deg->Write();

/*	g1->Draw();
	g2->Draw();
	g3->Draw();
	g4->Draw();
	g5->Draw();
 
	g1->Write();
	g2->Write();
	g3->Write();
	g4->Write();
	g5->Write();
*/	

	gStyle->SetOptFit(1011);

// 	 Destruyendo los punteros 
	finput->Close();
	output->Close();

	delete finput;
	delete output;
}
















