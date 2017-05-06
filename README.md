# weather-project
group 
//
//  ViewController.swift
//  Weather App
//
//  Created by se group on 3/12/17.
//  Copyright © 2017 se group. All rights reserved.
//

import UIKit

class ViewController: UIViewController {
    
    
    @IBOutlet weak var textField: UITextField!
    @IBOutlet weak var resultLabel: UILabel!
    
    @IBOutlet weak var Activity: UIActivityIndicatorView!
   
    @IBAction func searchButton(_ sender: AnyObject) {
        
        Activity.startAnimating()
        if ( textField.text?.isEmpty == false ){
            
            let textF = textField.text!.replacingOccurrences(of: " ", with: "-")
            
            let attemptedUrl = URL(string: "http://www.weather-forecast.com/locations/\(textF)/forecasts/latest")
            
            if let url = attemptedUrl {
            
            let task = URLSession.shared.dataTask(with: url, completionHandler: { (data, response, error) in
                
                let urlContent = data
                
                if urlContent != nil{
                    
                    let webContent = NSString(data: urlContent!, encoding: String.Encoding.utf8.rawValue)
                    
                    let webArray = webContent?.components(separatedBy: "3 Day Weather Forecast Summary:</b><span class=\"read-more-small\"><span class=\"read-more-content\"> <span class=\"phrase\">")
                    
                    if ( webArray!.count > 1){
                    
                    let weatherSummary = webArray![1].components(separatedBy: "</span>")
                    
                    let summary = weatherSummary[0].replacingOccurrences(of: "&deg;", with: "º")
                        
                    DispatchQueue.main.async(execute: {
                        
                        self.Activity.stopAnimating()
                        self.resultLabel.text = String(summary)
                        
                    })
                    
                        
                    }
                    else {
                        
                        self.Activity.stopAnimating()
                        self.resultLabel.textColor = UIColor.red
                        self.resultLabel.text = "Error Loading, Try again"
                    }
                    
                    
                    
                    
                }
                else {
                    
                    self.Activity.stopAnimating()
                    self.resultLabel.textColor = UIColor.red
                    self.resultLabel.text = "Error Loading, Please try again later"
                    
                    
                }
                
            }) 
            
            task.resume()
            
            }
                
            else {
                
                self.Activity.stopAnimating()
                self.resultLabel.textColor = UIColor.red
                resultLabel.text = "Unknown City - Please enter a valid city"
            }
            
        }
        else {
            
            self.Activity.stopAnimating()
            self.resultLabel.textColor = UIColor.red
            resultLabel.text = "City is Empty - Please enter a city"
        }
        
    }
    
    
    @IBAction func refreshButton(_ sender: AnyObject) {
        
        UIView.animate(withDuration: 1, animations: {
            self.viewL.center = CGPoint(x: self.viewL.center.x + 400, y: self.viewL.center.y)
            self.hideV.center = CGPoint(x: self.hideV.center.x + 400, y: self.hideV.center.y)
        }) 
    }
    
    @IBOutlet weak var viewL: UILabel!
    
    @IBAction func hideView(_ sender: AnyObject) {
        
        UIView.animate(withDuration: 0.5, animations: {
            self.viewL.center = CGPoint(x: self.viewL.center.x , y: self.viewL.center.y + 800)
            self.hideV.center = CGPoint(x: self.hideV.center.x , y: self.hideV.center.y + 800)
        }) 
    }
    
    @IBOutlet weak var hideV: UIButton!
    
    override func viewDidLayoutSubviews() {
        viewL.center = CGPoint(x: viewL.center.x - 400, y: viewL.center.y)
        hideV.center = CGPoint(x: hideV.center.x - 400, y: hideV.center.y)
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        Activity.stopAnimating()
        // Do any additional setup after loading the view, typically from a nib.
    }
    
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        self.view.endEditing(true)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}

