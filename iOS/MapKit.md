MapKit
===

## Create a class that conforms to both NSObject and MKAnnotation

~~~swift
import MapKit
import UIKit

class Capital: NSObject, MKAnnotation {
	var title:String?
	var coordinate: CLLocationCoordinate2D
	var info:String
	
	init(title:String, coordinate:CLLocationCoordinate2D, info:String){
		self.title = title
		self.coordinate = coordinate
		self.info = info
	}
}
~~~

## Add MKMapView to project

- inside ViewController

~~~swift
import MapKit
@IBOutlet weak var mapView:MKMapView!

func viewDidLoad(){
	self.mapView.delegate = self
}
~~~

### Add annotations to the MKMapView

~~~swift
self.mapView.addAnnotation(annotation: MKAnnotation)

self.mapView.addAnnotations(annotation: [MKAnnotation])
~~~

Note: W in longitude is a **negative** number.

***

## Add MKMapViewDelegate to display custom MKAnnotations

return `nil` when you aren't displaying your own custom MKAnnotation

~~~swift

func mapView(_ mapView:MKMapView, viewFor annotation: MKAnnotation) -> MKAnnotationView? {
	let identifier = "Capital" //so the MKAnnotation can be reused
	
	//If the selected annoation is of our custom type
	if annotation is Capital {
		var annotationView = mapView.dequeueReusableAnnotationView(withIdentifier: identifier)
            
           	if annotationView == nil {
                annotationView = MKPinAnnotationView(annotation: annotation, reuseIdentifier: identifier)
                annotationView!.canShowCallout = true
                
                let button = UIButton(type: .detailDisclosure)
                annotationView!.rightCalloutAccessoryView = button
            } else {
                annotationView!.annotation = annotation
            }
            
            return annotationView
        }
        
        return nil
}
~~~

### Function for displaying custom button

~~~swift
func mapView(_ mapView: MKMapView, annotationView view: MKAnnotationView, calloutAccessoryControlTapped control: UIControl) {
        let capital = view.annotation as! Capital
        let placeName = capital.title
        let placeInfo = capital.info
        
        let alertController = UIAlertController(title: placeName, message: placeInfo, preferredStyle: .alert)
        alertController.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
        self.present(alertController, animated: true, completion: nil)
    }
~~~