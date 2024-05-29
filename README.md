# React - Bootstrap Modal CURD Application

```js
import React, { useState } from 'react';
import './App.css';

import {
  FaUserPlus,
  FaPlus,
  FaRegUser,
  FaRegTimesCircle,
  FaUsers,
  FaTrashAlt,
  FaRegTrashAlt,
  FaRegEdit,
  FaRegSave,
  FaUserCheck,
} from 'react-icons/fa';

import Container from 'react-bootstrap/Container';
import Row from 'react-bootstrap/Row';
import Col from 'react-bootstrap/Col';
import Form from 'react-bootstrap/Form';
import Button from 'react-bootstrap/Button';
import Card from 'react-bootstrap/Card';
import Modal from 'react-bootstrap/Modal';
import Table from 'react-bootstrap/Table';

const userObj = {
  first_name: '',
  last_name: '',
  email_id: '',
  mobile_no: '',
  role: 'User',
  is_agreed: false,
};

const App = () => {
  const [isModalShow, setIsModalShow] = useState(false);
  const [user, setUser] = useState(userObj);
  const [userList, setUserList] = useState([]);
  const [isEditMode, setIsEditMode] = useState(false);
  const [editUserIndex, setEditUserIndex] = useState('');

  const userModalCloseHandler = () => {
    setIsModalShow(false);
    setIsEditMode(false);
    setEditUserIndex('');
  };

  const userModalShowHandler = () => {
    setIsModalShow(true);
  };

  const addUserHandler = () => {
    document.getElementById('userFormSubmitBtn').click();
  };

  const deleteUserHandler = (evt, index) => {
    evt.preventDefault();
    let _userList = [...userList];
    _userList.splice(index, 1);
    setUserList(_userList);
    //console.log(index);
  };

  const editUserHandler = (evt, index) => {
    evt.preventDefault();
    setUser(userList[index]);
    setEditUserIndex(index);
    setIsModalShow(true);
    setIsEditMode(true);
  };

  const updateUserHandler = () => {
    let _userList = [...userList];
    _userList[editUserIndex] = user;
    setUserList(_userList);
    setIsModalShow(false);
    let _userObj = {
      first_name: '',
      last_name: '',
      email_id: '',
      mobile_no: '',
      role: 'User',
      is_agreed: false,
    };
    setUser(_userObj);
    setIsEditMode(false);
    setEditUserIndex('');
  };

  const clearAllHandler = () => {
    setUserList([]);
    setIsEditMode(false);
    setEditUserIndex('');
  };

  const userFormSubmitHandler = (e) => {
    e.preventDefault();
    setUserList([...userList, user]);
    setIsModalShow(false);
    let _userObj = {
      first_name: '',
      last_name: '',
      email_id: '',
      mobile_no: '',
      role: 'User',
      is_agreed: false,
    };
    setUser(_userObj);
    setIsEditMode(false);
    setEditUserIndex('');
  };

  return (
    <>
      <Container fluid='md'>
        <Row className='mt-3'>
          <Col md={{ span: 10, offset: 1 }} xs={12}>
            <Card>
              <Card.Header>
                <Row>
                  <Col md={8}>
                    <h3>
                      <FaUsers style={{ marginTop: '-4px' }} /> User
                      Registration ({userList.length})
                    </h3>
                  </Col>
                  <Col md={4} style={{ textAlign: 'right' }}>
                    <Button variant='primary' onClick={userModalShowHandler}>
                      <FaUserPlus style={{ marginTop: '-4px' }} /> Add User
                    </Button>
                  </Col>
                </Row>
              </Card.Header>
              <Card.Body>
                <Table striped bordered hover variant='dark' responsive='sm'>
                  <thead>
                    <tr>
                      <th>SL</th>
                      <th>NAME</th>
                      <th>EMAIL</th>
                      <th>MOBILE</th>
                      <th>ROLE</th>
                      <th>AGREE</th>
                      <th style={{ width: '100px' }}>#</th>
                    </tr>
                  </thead>
                  <tbody>
                    {userList.length > 0 &&
                      userList.map((item, index) => {
                        return (
                          <tr key={'user-row-' + index}>
                            <td>{index + 1}</td>
                            <td>{item.first_name + ' ' + item.last_name}</td>
                            <td>{item.email_id}</td>
                            <td>{item.mobile_no}</td>
                            <td>{item.role}</td>
                            <td>{item.is_agreed ? 'YES' : 'NO'}</td>
                            <td>
                              <Button
                                type='button'
                                size='sm'
                                variant='outline-danger'
                                onClick={(e) => deleteUserHandler(e, index)}
                              >
                                <FaRegTrashAlt className='text-light' />
                              </Button>
                              <Button
                                type='button'
                                size='sm'
                                className='mx-2'
                                variant='outline-info'
                                onClick={(e) => editUserHandler(e, index)}
                              >
                                <FaRegEdit className='text-light' />
                              </Button>
                            </td>
                          </tr>
                        );
                      })}
                    {userList.length === 0 && (
                      <tr>
                        <td colSpan={7}>No records found!</td>
                      </tr>
                    )}
                  </tbody>
                </Table>
              </Card.Body>
              <Card.Footer style={{ textAlign: 'right' }}>
                <Button
                  type='button'
                  variant='danger'
                  onClick={clearAllHandler}
                  className={userList.length === 0 ? ' disabled' : ''}
                >
                  <FaTrashAlt style={{ marginTop: '-4px' }} /> Clear All
                </Button>
              </Card.Footer>
            </Card>
          </Col>
        </Row>

        <Modal
          size='md'
          show={isModalShow}
          onHide={userModalCloseHandler}
          backdrop='static'
          keyboard={false}
          aria-labelledby='contained-modal-title-vcenter'
          centered
        >
          <Modal.Header closeButton>
            <Modal.Title id='contained-modal-title-vcenter'>
              {isEditMode ? (
                <strong>
                  <FaUserCheck style={{ marginTop: '-4px' }} /> Edit User
                </strong>
              ) : (
                <strong>
                  <FaPlus style={{ marginTop: '-4px' }} /> Add User
                </strong>
              )}
            </Modal.Title>
          </Modal.Header>
          <Modal.Body>
            <Form id='addEditUserFrm' onSubmit={userFormSubmitHandler}>
              <Form.Group className='mb-3' controlId='newUserFirstName'>
                <Form.Label className='onex-frmlb'>
                  First Name: <em>*</em>
                </Form.Label>
                <Form.Control
                  type='text'
                  name='first_name'
                  placeholder='Enter First Name'
                  maxLength={30}
                  required
                  value={user.first_name}
                  onChange={(e) =>
                    setUser({ ...user, first_name: e.target.value })
                  }
                />
              </Form.Group>
              <Form.Group className='mb-3' controlId='newUserLastName'>
                <Form.Label className='onex-frmlb'>
                  Last Name: <em>*</em>
                </Form.Label>
                <Form.Control
                  type='text'
                  name='last_name'
                  placeholder='Enter Last Name'
                  maxLength={20}
                  required
                  value={user.last_name}
                  onChange={(e) =>
                    setUser({ ...user, last_name: e.target.value })
                  }
                />
              </Form.Group>
              <Form.Group className='mb-3' controlId='newUserEmail'>
                <Form.Label className='onex-frmlb'>
                  Email Id: <em>*</em>
                </Form.Label>
                <Form.Control
                  type='email'
                  name='email_id'
                  placeholder='Enter Email id'
                  maxLength={60}
                  required
                  value={user.email_id}
                  onChange={(e) =>
                    setUser({ ...user, email_id: e.target.value })
                  }
                />
              </Form.Group>
              <Form.Group className='mb-3' controlId='newUserMobile'>
                <Form.Label className='onex-frmlb'>
                  Mobile No: <em>*</em>
                </Form.Label>
                <Form.Control
                  type='tel'
                  name='mobile_no'
                  placeholder='Enter Mobile No'
                  minLength={10}
                  maxLength={12}
                  required
                  value={user.mobile_no}
                  onChange={(e) =>
                    setUser({ ...user, mobile_no: e.target.value })
                  }
                />
              </Form.Group>
              <Form.Group className='mb-3' controlId='newUserRole'>
                <Form.Label className='onex-frmlb'>
                  Role: <em>*</em>
                </Form.Label>
                <Form.Select
                  value={user.role}
                  onChange={(e) => setUser({ ...user, role: e.target.value })}
                >
                  <option value='User'>User</option>
                  <option value='Admin'>Admin</option>
                  <option value='Super-Admin'>Super Admin</option>
                </Form.Select>
              </Form.Group>
              <Form.Group className='mb-3' controlId='newUserAgree'>
                <Form.Check
                  type='switch'
                  id='custom-switch'
                  label='Yes, Agreed'
                  checked={user.is_agreed}
                  onChange={(e) =>
                    e.target.checked
                      ? setUser({ ...user, is_agreed: true })
                      : setUser({ ...user, is_agreed: false })
                  }
                />
              </Form.Group>
              <div>
                <button
                  type='submit'
                  id='userFormSubmitBtn'
                  className='d-none'
                ></button>
              </div>
            </Form>
          </Modal.Body>
          <Modal.Footer>
            <Button
              type='button'
              variant='danger'
              onClick={userModalCloseHandler}
            >
              <FaRegTimesCircle style={{ marginTop: '-4px' }} /> Close
            </Button>
            {isEditMode ? (
              <Button
                type='button'
                variant='success'
                onClick={updateUserHandler}
              >
                <FaRegSave style={{ marginTop: '-4px' }} /> Save Changes
              </Button>
            ) : (
              <Button type='button' variant='success' onClick={addUserHandler}>
                <FaRegUser style={{ marginTop: '-4px' }} /> Add User
              </Button>
            )}
          </Modal.Footer>
        </Modal>
      </Container>
    </>
  );
};

export default App;
```