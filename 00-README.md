Dans ce premier TP nous avons développé une API qui retourne au format JSON les headers de la requête quand il y une requête HTTP GET sur /ping.
Une réponse vide avec code 404 si quoi que ça soit d'autre que GET /ping
Le serveur doit écouter sur un port configurable via la variable d'environnement : PING_LISTEN_PORT ou par défaut sur un port au choix.

Le code pour cette API : 

use std::net::TcpListener;
use std::net::TcpStream;
use std::io::prelude::*;
use std::io::BufReader;
use std::io::BufRead;

fn main() {
    let listener = TcpListener::bind("0.0.0.0:7878").unwrap();

        for stream in listener.incoming() {
            let stream = stream.unwrap();

            handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream) {
    let buf_reader = BufReader::new(&mut stream);
    let httprequest: Vec<> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();


    if httprequest[0] == "GET /ping HTTP/1.1"{

            stream.write(format!("HTTP/1.1 200 OK \r\nContent-Type: application/json\r\n\n{}",json (httprequest)).as_bytes()).unwrap();
    }
    else {
        let status_line = "HTTP/1.1 404 NOT FOUND\r\n";
        stream.write_all(status_line.as_bytes()).unwrap();
    }
}

fn json (http_request:Vec) -> String{
    let mut json = String::new();
    let mut http_request = http_request;

    http_request.remove([0]);
}


