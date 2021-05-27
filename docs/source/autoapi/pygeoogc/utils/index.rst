:mod:`pygeoogc.utils`
=====================

.. py:module:: pygeoogc.utils

.. autoapi-nested-parse::

   Some utilities for PyGeoOGC.



Module Contents
---------------

.. py:class:: ESRIGeomQuery(geometry: Union[Tuple[float, float], List[Tuple[float, float]], Tuple[float, float, float, float], Polygon], wkid: int)

   Generate input geometry query for ArcGIS RESTful services.

   :Parameters: * **geometry** (:class:`tuple` or :class:`Polygon`) -- The input geometry which can be a point (x, y), a list of points [(x, y), ...],
                  bbox (xmin, ymin, xmax, ymax), or a Shapely's Polygon.
                * **wkid** (:class:`int`) -- The Well-known ID (WKID) of the geometry's spatial reference e.g., for EPSG:4326,
                  4326 should be passed. Check
                  `ArcGIS <https://developers.arcgis.com/rest/services-reference/geographic-coordinate-systems.htm>`__
                  for reference.

   .. method:: bbox(self) -> Dict[str, Union[str, bytes]]

      Query for a bbox.


   .. method:: get_payload(self, geo_type: str, geo_json: Dict[str, Any]) -> Dict[str, Union[str, bytes]]

      Generate a request payload based on ESRI template.

      :Parameters: * **geo_type** (:class:`str`) -- Type of the input geometry
                   * **geo_json** (:class:`dict`) -- Geometry in GeoJson format.

      :returns: :class:`dict` -- An ESRI geometry payload.


   .. method:: multipoint(self) -> Dict[str, Union[str, bytes]]

      Query for a multi-point.


   .. method:: point(self) -> Dict[str, Union[str, bytes]]

      Query for a point.


   .. method:: polygon(self) -> Dict[str, Union[str, bytes]]

      Query for a polygon.



.. py:class:: MatchCRS

   Match CRS of a input geometry (Polygon, bbox, coord) with the output CRS.

   :Parameters: * **geometry** (:class:`tuple` or :class:`Polygon`) -- The input geometry (Polygon, bbox, coord)
                * **in_crs** (:class:`str`) -- The spatial reference of the input geometry
                * **out_crs** (:class:`str`) -- The target spatial reference

   .. method:: bounds(geom: Tuple[float, float, float, float], in_crs: str, out_crs: str) -> Tuple[float, float, float, float]
      :staticmethod:

      Reproject a bounding box to the specified output CRS.


   .. method:: coords(geom: Tuple[Tuple[float, ...], Tuple[float, ...]], in_crs: str, out_crs: str) -> Tuple[Any, ...]
      :staticmethod:

      Reproject a list of coordinates to the specified output CRS.


   .. method:: geometry(geom: Union[Polygon, MultiPolygon, Point, MultiPoint], in_crs: str, out_crs: str) -> Union[Polygon, MultiPolygon, Point, MultiPoint]
      :staticmethod:

      Reproject a geometry to the specified output CRS.



.. py:class:: RetrySession(retries: int = 3, backoff_factor: float = 0.3, status_to_retry: Tuple[int, ...] = (500, 502, 504), prefixes: Tuple[str, ...] = ('https://', ))

   Configures the passed-in session to retry on failed requests.

   The fails can be due to connection errors, specific HTTP response
   codes and 30X redirections. The code is based on:
   https://github.com/bustawin/retry-requests

   :Parameters: * **retries** (:class:`int`, *optional*) -- The number of maximum retries before raising an exception, defaults to 5.
                * **backoff_factor** (:class:`float`, *optional*) -- A factor used to compute the waiting time between retries, defaults to 0.5.
                * **status_to_retry** (:class:`tuple`, *optional*) -- A tuple of status codes that trigger the reply behaviour, defaults to (500, 502, 504).
                * **prefixes** (:class:`tuple`, *optional*) -- The prefixes to consider, defaults to ("http://", "https://")

   .. method:: get(self, url: str, payload: Optional[Mapping[str, Any]] = None, headers: Optional[MutableMapping[str, Any]] = None) -> Response

      Retrieve data from a url by GET and return the Response.


   .. method:: onlyipv4() -> _patch
      :staticmethod:

      Disable IPv6 and only use IPv4.


   .. method:: post(self, url: str, payload: Optional[MutableMapping[str, Any]] = None, headers: Optional[MutableMapping[str, Any]] = None) -> Response

      Retrieve data from a url by POST and return the Response.



.. function:: async_requests(url_payload: List[Tuple[str, Optional[MutableMapping[str, Any]]]], read: str, request: str = 'GET', max_workers: int = 8) -> List[Union[str, MutableMapping[str, Any], bytes]]

   Send async requests.

   This function is based on
   `this <https://github.com/HydrologicEngineeringCenter/data-retrieval-scripts/blob/master/qpe_async_download.py>`__
   script.

   :Parameters: * **url_payload** (:class:`list` of :class:`tuples`) -- A list of URLs and payloads as a tuple.
                * **read** (:class:`str`) -- The method for returning the request; binary, json, and text.
                * **request** (:class:`str`, *optional*) -- The request type; GET or POST, defaults to GET.
                * **max_workers** (:class:`int`, *optional*) -- The maximum number of async processes, defaults to 8.

   :returns: :class:`list` -- A list of responses


.. function:: bbox_decompose(bbox: Tuple[float, float, float, float], resolution: float, box_crs: str = DEF_CRS, max_px: int = 8000000) -> List[Tuple[Tuple[float, float, float, float], str, int, int]]

   Split the bounding box vertically for WMS requests.

   :Parameters: * **bbox** (:class:`tuple`) -- A bounding box; (west, south, east, north)
                * **resolution** (:class:`float`) -- The target resolution for a WMS request in meters.
                * **box_crs** (:class:`str`, *optional*) -- The spatial reference of the input bbox, default to EPSG:4326.
                * **max_px** (:class:`int`, :class:`opitonal`) -- The maximum allowable number of pixels (width x height) for a WMS requests,
                  defaults to 8 million based on some trial-and-error.

   :returns: :class:`tuple` -- The first element is a list of bboxes and the second one is width of the last bbox


.. function:: bbox_resolution(bbox: Tuple[float, float, float, float], resolution: float, bbox_crs: str = DEF_CRS) -> Tuple[int, int]

   Image size of a bounding box WGS84 for a given resolution in meters.

   :Parameters: * **bbox** (:class:`tuple`) -- A bounding box in WGS84 (west, south, east, north)
                * **resolution** (:class:`float`) -- The resolution in meters
                * **bbox_crs** (:class:`str`, *optional*) -- The spatial reference of the input bbox, default to EPSG:4326.

   :returns: :class:`tuple` -- The width and height of the image


.. function:: check_bbox(bbox: Tuple[float, float, float, float]) -> None

   Check if an input inbox is a tuple of length 4.


.. function:: check_response(resp: Response) -> None

   Check if a ``requests.Resonse`` returned an error message.


.. function:: threading(func: Callable, iter_list: Iterable, param_list: Optional[List[Any]] = None, max_workers: int = 8) -> List[Any]

   Run a function in parallel with threading.

   .. rubric:: Notes

   This function is suitable for IO intensive functions.

   :Parameters: * **func** (:class:`function`) -- The function to be ran in threads
                * **iter_list** (:class:`list`) -- The iterable for the function
                * **param_list** (:class:`list`, *optional*) -- List of other parameters, defaults to an empty list
                * **max_workers** (:class:`int`, *optional*) -- Maximum number of threads, defaults to 8

   :returns: :class:`list` -- A list of function returns for each iterable. The list is not ordered.


.. function:: traverse_json(obj: Union[Dict[str, Any], List[Dict[str, Any]]], path: Union[str, List[str]]) -> List[Any]

   Extract an element from a JSON file along a specified path.

   This function is based on `bcmullins <https://bcmullins.github.io/parsing-json-python/>`__.

   :Parameters: * **obj** (:class:`dict`) -- The input json dictionary
                * **path** (:class:`list`) -- The path to the requested element

   :returns: :class:`list` -- The items founds in the JSON

