#uni 

Ogni VLAN ha un unico dominio broadcast, quindi host su diverse VLAN non possono comunicare.
Questo risolve il problema della non comunicazione fra host a livello due ma vorremmo che a livello 3 i dispositivi su diverse VLAN possano comunicare lo stesso. 

Per fare ciò ho bisogno che le VLAN separata abbiano il proprio spazio di indirizzamento separato e disgiunto. Ogni VLAN avrà quindi un indirizzo IP **unico**.

# Port-based VLAN 
Una prima metodologia è quella di utilizzare un router *esterno* che prevede una porta per ogni VLAN capace quindi di indirizzare il traffico fra esse. 
Questo meccanismo si basa sul **port-based VLAN** che funziona solo se vi è una singolo VLAN per porta.
Per il Router quindi l'archittettura di rete sottostante è trasparente, se ne occuperanno gli switch che dovranno essere configurati coerentemente.

# Router on a Stick
Il port-based VLAN è una soluzione non scalabile in quanto richede una porta attiva per ogni VLAN. Per ovviare a questo problema si spinge sul contetto di *virtualizzazione*. 
L'idea è quella di avere un **one arm router**, ovver un router che una sola porta attiva, che si comporta come se avesse un interfaccia fisica configurata come trunk alla quale sono collegate $N$ interfaccia *virtuali*, una per ogni VLAN. 

# Switch multilivello
La suluzione più gettonata è quella di utilizzare uno **switch multlilivello**. Di fatto quello che stavamo cercando di fare era combinare in un solo dispositivo le funzionalità dello switch e del router. Oggigiorno la differenza hardware fra dispositivi di livello 2 e livello 3 si è assottigliata molto; è molto comune trovare gli switch che operano anche a livello 3.




