//This is an Apex Class extension that uses a visualforce page to insertcreate an object name out of an individual's first and last names. Title "ParticipantControllerExt"


public with sharing class ParticipantControllerExt {
    private final Participant__c par;
    
    public ParticipantControllerExt (ApexPages.StandardController stdController) {
        this.par = (Participant__c)stdController.getRecord();
    }

    public void setName() {
        if(par.Name == null) {
            par.put('Name', 'No Name');
        }
    }
    
    static testMethod void participantEmployerContactTest() {
        Participant__c testPar = new Participant__c(Name='none', First_Name__c = 'tEsT', Middle_Initial__c = 'a', Last_Name__c = 'pArTiciPant');
        insert testPar;
        
        Employer__c testEmp = new Employer__c(Name='Test Employer');
        insert testEmp;
        
        Employer__c insertEmp = [SELECT Id FROM Employer__c WHERE Name = 'Test Employer'];
        
        Employer_Contact__c testEmpCon = new Employer_Contact__c(First_Name__c='Test Employer Con', Employer__c = insertEmp.Id);
        insert testEmpCon;
        
        Test.SetCurrentPageReference(New PageReference('Page.ParticipantEdit'));
        ApexPages.StandardController stdCont = new ApexPages.StandardController(testPar);
        ParticipantControllerExt contExt = new ParticipantControllerExt(stdCont);
        contExt.setName();
        
        update testPar;
        
        Participant__c resultPar = [SELECT Name FROM Participant__c WHERE ID =: testPar.Id];
        system.Assert((String)resultPar.Name == 'Test A Participant');
    }
} 
*/
