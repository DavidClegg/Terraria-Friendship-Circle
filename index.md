<style>
    body{
        background-color: #222;
        display:flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        font-family: sans-serif;
        font-size: 20px;
    }
    svg{
        background-color: #222;
        width: 80vw;
        height: 80vh;
    }
    text{
        font: bold 0.1px serif;
    }
    .pg{
        transform: rotate(10deg);
        filter: hue-rotate(45deg);
    }
</style>

<body>
    <!--

        <svg id=viewBox ></svg>
    -->
    <p style="color:crimson">Red = Dislike</p>
    <p style="color:chartreuse">Green = Like</p>
    <p style="color:hotpink">Pink = Love</p>
</body>

<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<!-- I added jQuery super late into to the project so I only used it for like one thing :( -->
<script>
    let body = document.getElementsByTagName("body")[0]
    let viewBox = document.createElementNS("http://www.w3.org/2000/svg","svg")
    viewBox.setAttribute("id","viewBox");
    let ref = body.getElementsByTagName("p")[0]
    body.insertBefore(viewBox, ref)
    //body.appendChild(viewBox)
    viewBox = document.getElementById("viewBox");
    let size = 5; // Change the viewbox size and the size here to change the size of the grid
    viewBox.setAttribute("viewBox", `0 0 ${size} ${size}`);


    const radianTest = (degrees) => {
        let radius = 1;
        let radians = degrees * Math.PI / 180;
        let x = Math.cos(radians)/radius;
        let y = Math.sin(radians)/radius;

        let point = document.createElementNS("http://www.w3.org/2000/svg","circle");
        point.setAttribute("cx", x + (size/2) );
        point.setAttribute("cy", y + (size/2) );
        point.setAttribute("r",.1);
        point.setAttribute("fill","chartreuse");
        viewBox.appendChild(point);
    }

    const radianText = (degrees) => {
        let radius = 1;
        let radians = degrees * Math.PI / 180;
        let x = Math.cos(radians)/radius;
        let y = Math.sin(radians)/radius;

        let label = document.createElementNS("http://www.w3.org/2000/svg","text");
        label.setAttribute("x", x + (size/2) );
        label.setAttribute("y", y + (size/2) );
        label.textContent = (degrees/360)*25;
        label.setAttribute("fill","black");
        viewBox.appendChild(label);
    }

    const drawLables = (n) => {
        for(let p = 0; p < n; p++){
            radianText(p * (360 /n))
        }
    }

    const drawNPoints = (n) => {
        for(let p = 0; p < n; p++){
            radianTest(p * (360 /n))
        }
    }

    const drawCenter = (sizeVariable) => {
        let point = document.createElementNS("http://www.w3.org/2000/svg","circle");
        point.setAttribute("cx", sizeVariable/2 );
        point.setAttribute("cy", sizeVariable/2 );
        point.setAttribute("r", 0.1);
        point.setAttribute("fill","gold");
        viewBox.appendChild(point);
    }

    const drawLineByDegree = (points, start, end, color) => {
        let radians = (start * (360 / points) ) * Math.PI / 180;
        let startX = Math.cos(radians) + (size/2);
        let startY = Math.sin(radians) + (size/2);
        
        radians = (end * (360 / points) ) * Math.PI / 180;
        let endX = Math.cos(radians) + (size/2);
        let endY = Math.sin(radians) + (size/2);
       
        let midX = (size/2);
        let midY = (size/2);


        let magX = startX - (startX - endX)/2;
        magX = magX - (magX - midX)/2;
        let magY = startY - (startY - endY)/2;
        magY = magY - (magY - midY)/2;

        let point = document.createElementNS("http://www.w3.org/2000/svg","circle");
        point.setAttribute("cx",magX);
        point.setAttribute("cy",magY);
        point.setAttribute("r",.1);
        point.setAttribute("fill","#0ff");
        //viewBox.appendChild(point)

        let line = document.createElementNS("http://www.w3.org/2000/svg","path");
        line.setAttribute("d",`M${startX} ${startY} Q ${magX} ${magY} ${endX} ${endY}`);
        // Swap the magX and magY for midX and midY to remove the magnitude scaling that I worked so hard to apply. 
        line.setAttribute("stroke",color);
        line.setAttribute("class","line");
        line.setAttribute("stroke-width",0.05);
        line.setAttribute("fill","none")
        viewBox.appendChild(line);
    }

    const drawBox = (points, start, length, colour) => {

        //radianTest(start * (360/25));
        //radianTest((start + length) * (360/25));
        start -= 1.5
        length -= 0.05
        let radius = (size/2);
        let offset = 0.65;
        let radiansAD = (start * (360 / points) ) * Math.PI / 180;
        let aX = Math.cos(radiansAD)/1.05 + radius;
        let aY = Math.sin(radiansAD)/1.05 + radius;
        let dX = (Math.cos(radiansAD)/offset) + radius;
        let dY = (Math.sin(radiansAD)/offset) + radius;

        radiansBC = ((start + length) * (360 / points)) * Math.PI / 180;
        let bX = Math.cos(radiansBC)/1.05 + radius;
        let bY = Math.sin(radiansBC)/1.05 + radius;
        let cX = (Math.cos(radiansBC)/offset) + radius;
        let cY = (Math.sin(radiansBC)/offset) + radius;
        
        // Maybe I could use the little q and change my math slightly to a relative offset instead of an absolute offset?
        radiansQ = ((start + (length/2)) * (360 / points)) * Math.PI / 180;
        let abX = Math.cos(radiansQ)/1.05 + radius;
        let abY = Math.sin(radiansQ)/1.05 + radius;
        let cdX = (Math.cos(radiansQ)/offset) + radius;
        let cdY = (Math.sin(radiansQ)/offset) + radius;
        console.log(aX)
        //radianTest((start + (length/2)) * (360/25));

        console.log(
            ` Points:
            a:${aX},${aY}
            b:${bX},${bY}
            c:${cX},${cY}
            d:${dX},${dY}
            Q1:${abX}, ${abY}
            Q2:${cdX}, ${cdY}
            `
        )
        
        //let aX; let aY; // initial point, based on start
        //let bX; let bY; // some number of points away from [a]
        //let cX; let cY; // some const width from [b]
        //let dX; let dY; // some const width from [a]
        

        let line = document.createElementNS("http://www.w3.org/2000/svg","path");
        line.setAttribute("d",`M${aX} ${aY} Q${abX} ${abY} ${bX} ${bY} L${cX} ${cY}Q${cdX} ${cdY} ${dX} ${dY}L${aX} ${aY}`);
        line.setAttribute("onclick",`drawNPC(npcs[${parseInt(start+0.5)}])`)
        line.setAttribute("stroke","none");
        line.setAttribute("stroke-width",0.01);
        line.setAttribute("fill",colour)
        viewBox.appendChild(line);
    };

    /*
        Okay, so. 
        To make the boxes I could use a path:
            a b  do Q from a to b, line b to d
            c d  do Q from d to c, line c to a
    */

    //drawCenter(size);
    //drawNPoints(25);
    //drawLables(25);

    for(let i = 0; i < 25; i++){
        let color;
        switch(i){
        case 24:
        case 0:
        case 1:
        case 2:
            color = "aquamarine"; break;
        case 3:
        case 4:
        case 5: 
            color = "olivedrab"; break;
        case 6:
        case 7:
        case 8: 
            color = "deepskyblue"; break;
        case 9:
        case 10:
        case 11: 
            color = "gold"; break;
        case 12:
        case 13:
        case 14: 
            color = "slategrey"; break;
        case 15: 
            color = "royalblue"; break;
        case 16:
        case 17:
        case 18:
        case 19: 
            color = "forestgreen"; break;
        case 20:
        case 21:
        case 22:
        case 23: 
            color = "pink"; break;
        }
        drawBox(25, i, 1, color);
    }

    let love = "hotpink";
    let like = "springgreen";
    let dislike = "crimson";
    //drawLineByDegree(25, 5, 1, dislike);
    //drawLineByDegree(25, 12, 1, like);
    //drawLineByDegree(25, 1, 3, love);

    function NPC (loves, likes, dislikes, me, name, imgUrl){
        this.loves = loves,
        this.likes = likes,
        this.dislikes = dislikes,
        this.me = me,
        this.name = name,
        this.imgUrl = imgUrl
    }

    const npcs = {
        // Love, Like, Hate, Me
        // Snow part 2
        0:mechanic = new NPC ( [11], [24], [10], 0, "Mechanic","https://gamepedia.cursecdn.com/terraria_gamepedia/5/55/Mechanic.png?version=ee3e7ebf0b4fe821b1387a79fef7fba5"),
        1:santa = new NPC ( [], [], [], 1, "Santa", "https://gamepedia.cursecdn.com/terraria_gamepedia/5/58/Santa_Claus.png?version=82d138f0b0f830225b3ed988612ff8a8"), // none
        // Jungle
        2:painter = new NPC ( [3], [21], [14, 24], 2, "Painter","https://gamepedia.cursecdn.com/terraria_gamepedia/2/24/Painter.png?version=c7deb852cb9bd6953ab12c9c6075f49e"),
        3:dryad = new NPC ( [], [4, 14], [5], 3, "Dryad","https://gamepedia.cursecdn.com/terraria_gamepedia/5/5c/Dryad.png?version=d7811816a9b3908aec153990d5616255"),
        4:witch_doctor = new NPC ( [], [3, 15], [19], 4, "Witch Doctor","https://gamepedia.cursecdn.com/terraria_gamepedia/a/ac/Witch_Doctor.png?version=4dbe2d779343bed91b7e6aa335774d42"),
        // Ocean
        5:angler = new NPC ( [], [12, 21, 23], [], 5, "Angler","https://gamepedia.cursecdn.com/terraria_gamepedia/b/bf/Angler.png?version=e4c31b2d8bed57a0240f6c5179434262"),
        6:pirate = new NPC ( [5], [20], [7], 6, "Pirate","https://gamepedia.cursecdn.com/terraria_gamepedia/7/7d/Pirate.png?version=f6e620facfc8a054f45020908b09951d"),
        7:stylist = new NPC ( [8], [6], [20], 7, "Stylist","https://gamepedia.cursecdn.com/terraria_gamepedia/1/16/Stylist.png?version=a524282e9c37f916526f5acf0d407444"),
        // Desert
        8:dye_trader = new NPC ( [], [10, 2], [9], 8, "Dye Trader","https://gamepedia.cursecdn.com/terraria_gamepedia/5/51/Dye_Trader.png?version=195b01ae30357dbf07b3bce964929155"),
        9:steampunker = new NPC ( [24], [2], [3, 22, 21], 9, "Steampunker","https://gamepedia.cursecdn.com/terraria_gamepedia/8/82/Steampunker.png?version=b3ce3587608df731e38a41756aafbac7"),
        10:arms_dealer = new NPC ( [19], [9], [17], 10, "Arms Dealer","https://gamepedia.cursecdn.com/terraria_gamepedia/9/9e/Arms_Dealer.png?version=d05689f02d0f1bb54d5e256bf2393f9b"),
        // Underground
        11:goblin_tinkerer = new NPC ( [0], [8], [13], 11, "Goblin Tinkerer","https://gamepedia.cursecdn.com/terraria_gamepedia/8/86/Goblin_Tinkerer.png?version=4bda2717de60d6f401487393ba8c83e9"),
        12:demolitionist = new NPC ( [20], [0], [10, 11], 12, "Demolitionist","https://gamepedia.cursecdn.com/terraria_gamepedia/6/6e/Demolitionist.png?version=540a13e6746b5ba60c1b32056f9d7883"),
        13:clothier = new NPC ( [14], [23], [19], 13, "Clothier","https://gamepedia.cursecdn.com/terraria_gamepedia/d/d2/Clothier.png?version=e323c5e5983510225a5955c71c187a9a"),
        // Mushroom
        14:truffle = new NPC ( [15], [3], [13], 14, "Truffle","https://gamepedia.cursecdn.com/terraria_gamepedia/f/f2/Truffle.png?version=cc94bd78316883d268b7c1285feb1f99"),
        // Forest
        15:guide = new NPC ( [], [13, 16], [9], 15, "Guide","https://gamepedia.cursecdn.com/terraria_gamepedia/7/7f/Guide.png?version=4e4e0ae76bcfff5ba3496aa221010f76"),
        16:zoologist = new NPC ( [4], [17], [5], 16, "Zoologist",'https://gamepedia.cursecdn.com/terraria_gamepedia/6/61/Zoologist.png?version=efcb0a55c160ca3b92a0251fe4285957'),
        17:golfer = new NPC ( [5], [2, 16], [6], 17, "Golfer",'https://gamepedia.cursecdn.com/terraria_gamepedia/1/1a/Golfer.png?version=7c6d06e1b7f9f3396e80dbd5f20ef6ba'),
        18:merchant = new NPC ( [], [17, 19], [23], 18, "Merchant",'https://gamepedia.cursecdn.com/terraria_gamepedia/1/19/Merchant.png?version=56631d5fcd37a112c1214e79f5844071'),
        // Hallow
        19:nurse = new NPC ( [10], [22], [3, 21], 19, "Nurse",'https://gamepedia.cursecdn.com/terraria_gamepedia/c/cc/Nurse.png?version=6592bec50bccfdeed4636d976baa1376'),      
        20:tavernkeep = new NPC ( [12], [11], [15], 20, "Taverkeep",'https://gamepedia.cursecdn.com/terraria_gamepedia/8/81/Tavernkeep.png?version=7754c9355e80f4c2284284f111d119b7'),
        21:party_girl = new NPC ( [22, 16], [7], [18], 21, "Party Girl", "https://gamepedia.cursecdn.com/terraria_gamepedia/a/a8/Party_Girl.png?version=73d4b3b98bcc5cd10174c85e804fbacb"),
        22:wizard = new NPC ( [17], [18], [4], 22, "Wizard",'https://gamepedia.cursecdn.com/terraria_gamepedia/c/c7/Wizard.png?version=b24ce40767d79a44c648433664fa454c'),
        // Snow part 1
        23:tax_collector = new NPC ( [18], [21], [12, 0], 23, "Tax Collector",'https://gamepedia.cursecdn.com/terraria_gamepedia/7/75/Tax_Collector.png?version=2f05b35625f5869b7cd402f8f8cb89c6'),
        24:cyborg = new NPC ( [], [9, 6, 7], [16], 24, "Cyborg",'https://gamepedia.cursecdn.com/terraria_gamepedia/a/a3/Cyborg.png?version=2571cdd2f182449775a8784e7d9a9d27'),
    };

    const drawNPC = (npc) => {
        $(".line").remove()

        console.log(`NPC: ${npc.name}, ${npc.me}
LOVE:${npc.loves}
LIKE:${npc.likes}
DISL:${npc.dislikes}`)
        if(npc.loves.length > 0){
            for(let lover of npc.loves){
                drawLineByDegree(25, npc.me, lover, love)
            }
        }
        if(npc.likes.length > 0){
            for(let liker of npc.likes){
                drawLineByDegree(25, npc.me, liker, like)
            }
        }
        if(npc.dislikes.length > 0){
            for(let hater of npc.dislikes){
                drawLineByDegree(25, npc.me, hater, dislike)
            }
        }
    };

// OKAY. SO NOW I NEED TO ADD THE OTHER ASSETS AND BREAK EVERYTHING!

// How do I even integerate the images into the svg? Is that a thing?
// It is a think

// For all of npcs{ draw all.imgUrl } 

for(let character in npcs){
    let npc = document.createElementNS("http://www.w3.org/2000/svg","image");
    npc.setAttribute("href",npcs[character].imgUrl);
    npc.setAttribute("x",size/2.2);
    npc.setAttribute("y",1.0);
    npc.setAttribute("width", .30);
    npc.setAttribute("height", .44);
    npc.setAttribute("transform-origin","50% 50%");
    npc.setAttribute("style",`transform:rotate(calc((360deg /25)*(${6.45+parseFloat(character)}))`);
    npc.setAttribute("onclick",`drawNPC(npcs[${parseInt(character)}])`);
    viewBox.appendChild(npc);    
}

/*let pgl = npcs[0].imgUrl;
let pg = document.createElementNS("http://www.w3.org/2000/svg","image");
pg.setAttribute("href",pgl);
pg.setAttribute("x",size/2.2);
pg.setAttribute("y",1.);
pg.setAttribute("width", .30);
pg.setAttribute("height", .44);
pg.setAttribute("class","pg");
pg.setAttribute("transform-origin","50% 50%");
pg.setAttribute("style","transform:rotate(calc((360deg /25)*6.45 ))");
pg.setAttribute("onclick","drawNPC(npcs[22])");

viewBox.appendChild(pg);// This is all together a better method for what I was doing.*/
    
</script>
