#Prace se skautisem
Když je uživatel přihlášen, je vše připraveno k posílání dotazů na servery SkautISu. SkautIS používá pro komunikaci [SOAP](http://cs.wikipedia.org/wiki/SOAP) protokol a k dispozici jsou tyto [služby](http://test-is.skaut.cz/JunakWebservice/).

##Jak vypadá dotaz na server.
###Objekt webové služby
Jednotlivé služby mají vlastní objekt splňující ``Skautis\Wsdl\WebServiceInterface``. Tento objekt je potřeba získat z objektu knihovny. Tento objekt se dá používat nadále samostatně v aplikaci.
Vyzkoušet si jak vypadají požadavky a odpovědi lze online na [ws.skautis.cz/testovani](http://ws.skautis.cz/testovani/).
```php
//$skautis je nakonfigurovaná knihovna s přihlášeným uživatelem

//V seznamu služeb si najdu jmeno služby kterou chci použít a získám její objekt
//$sluzba = $skautis->nazev_webove_sluzby;
//Pro práci s jednotkami ve  skautisu existuje služba OrganizationUnit
$organizationUnit = $skautis->OrganizationUnit; // Skautis\Wsd\WebServiceInterface

//Na webove sluzbe se provádějí akce. Tyto akce zpravidla mají nějaké parametry které se zadávají pomocí asociativního pole
//$params = ["nazev_atributu" => "hodnota_atributu"]
//Například filtrovat podle nadřazené jednotky.
$params = ["ID_UnitParent " => "24404"]

//Provedení akce se dá provést dvouma způsoby jediný rozdíl je v zápisu
//$data = $sluzba->jmeno_funkce($params);
//$data = $sluzba->call('jmeno_funkce', $params);

$data = $organizationUnit->unitAll($params);
$data = $organizationUnit->call('unitAll', $params);
```

###Aliasy webových sluzeb
Názvy služeb jsou dlouhé člověk je nechce psát stále dokola. Proto pro přístup k webové službě skautisu můžeme použít alias webové služby.

Již předdefinované aliasy jsou k dispozici:

* usr => user => UserManagement
* org => OrganizationUnit
* app => ApplicationManagement
* event => events => Events

Tyto tři příkazy jsou naprosto ekvivalentní.
```php
//UserManagement je webová služba pro práci s uživateli SkautISu
//UserDetail je akce na službě UserManagement k získání informací o uživateli
//1940 je ID uzivatele okres blansko

//Naprosto stejné požadavky
$data = $skautis->UserManagement->UserDetail(array("ID"=>1940));
$data = $skautis->user->UserDetail(array("ID"=>1940));
$data = $skautis->usr->UserDetail(array("ID"=>1940));
```

[Best practices](./best_practices.md)
